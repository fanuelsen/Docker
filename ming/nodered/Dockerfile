FROM alpine:3
MAINTAINER Per-Ole Fanuelsen

RUN set -xe \
    && apk add --no-cache \
        bash \
        build-base \
        ca-certificates \
        curl \
        nodejs \
        nodejs-npm \
        python3 \
        python3-dev \
    && ln -sf /usr/bin/python3 /usr/bin/python \
    && npm install -g --unsafe-perm \
        node-red \
        node-red-admin \
        node-red-dashboard \
        node-red-contrib-influxdb \
        node-red-contrib-stackhero-influxdb-v2 \
    && apk del \
        build-base \
        python3-dev \
    && rm -rf /tmp/npm-*

WORKDIR /data
VOLUME /data

EXPOSE 1880

CMD ["node-red", "--userDir", "/data", "--flowFile", "flows.json"]