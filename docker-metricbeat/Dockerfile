# Dockerfile for Metricbeat

# adapted from github.com/nguoianphu/docker-filebeat
# by gotjoshua

# Build with:
# docker build -t "metricbeat" .

# Alpine OS 3.4
# http://dl-cdn.alpinelinux.org/alpine/v3.4/community/x86_64/
FROM alpine:3.4

#PREVIOUS MAINTAINER Tuan Vo <vohungtuan@gmail.com>
MAINTAINER gotjoshua <me@1m.in>

###############################################################################
#                                INSTALLATION
###############################################################################

ARG BEAT_VERSION=5.6.3 
#note: can be trumped from docker-compose:
ENV BEAT_VERSION=$BEAT_VERSION


RUN set -x \
 && apk add --update bash \
                     curl \
                     tar \
                     openssl \
 && apk --no-cache add ca-certificates \
 && wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://raw.githubusercontent.com/sgerrand/alpine-pkg-glibc/master/sgerrand.rsa.pub \
 && wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.23-r3/glibc-2.23-r3.apk \
 && apk add glibc-2.23-r3.apk \
 && rm glibc-2.23-r3.apk

RUN curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-${BEAT_VERSION}-linux-x86_64.tar.gz \
 && tar xzvf metricbeat-${BEAT_VERSION}-linux-x86_64.tar.gz -C / --strip-components=1 \
 && rm -rf metricbeat-${BEAT_VERSION}-linux-x86_64.tar.gz \
 && apk del curl \
            tar \
            openssl \
 && rm -rf /var/cache/apk/*

###############################################################################
#                                   START
###############################################################################

COPY docker-entrypoint.sh /
RUN chmod +x docker-entrypoint.sh metricbeat \
 && ln -s /metricbeat /bin/metricbeat

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD []
