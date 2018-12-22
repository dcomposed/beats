# Dockerfile for Filebeat

# adapted from github.com/nguoianphu/docker-filebeat
# by gotjoshua

# Build with:
# docker build -t "filebeat" .

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
 && apk update && apk --no-cache add ca-certificates wget && update-ca-certificates \
 && wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub \
 && wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.28-r0/glibc-2.28-r0.apk \
 && apk add glibc-2.28-r0.apk \
 && rm glibc-2.28-r0.apk

RUN curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-${BEAT_VERSION}-linux-x86_64.tar.gz \
 && tar xzvf filebeat-${BEAT_VERSION}-linux-x86_64.tar.gz -C / --strip-components=1 \
 && rm -rf filebeat-${BEAT_VERSION}-linux-x86_64.tar.gz \
 && apk del curl \
            tar \
            openssl \
 && rm -rf /var/cache/apk/*

###############################################################################
#                                   START
###############################################################################

COPY docker-entrypoint.sh /
RUN chmod +x docker-entrypoint.sh filebeat \
 && ln -s /filebeat /bin/filebeat

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD []
