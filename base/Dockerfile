FROM docker.io/library/alpine:edge

ARG TARGETPLATFORM
ARG GITHUB_TOKEN
ARG INSTALL_FFMPEG=false
ARG INSTALL_ARIA2=false
LABEL MAINTAINER="OpenList"

WORKDIR /opt/openlist/

RUN apk update && \
    apk upgrade --no-cache && \
    apk add --no-cache bash ca-certificates su-exec tzdata runit; \
    [ "$INSTALL_FFMPEG" = "true" ] && apk add --no-cache ffmpeg; \
    [ "$INSTALL_ARIA2" = "true" ] && apk add --no-cache curl aria2 && \
        mkdir -p /opt/aria2/.aria2 && \
        curl -fsSL --retry 3 -H "Authorization: Bearer $GITHUB_TOKEN" https://github.com/P3TERX/aria2.conf/archive/refs/heads/master.tar.gz -O /tmp/aria-conf.tar.gz && \
        tar -zxvf /tmp/aria-conf.tar.gz -C /opt/aria2/.aria2 --strip-components=1 && rm -f /tmp/aria-conf.tar.gz && \
        sed -i 's|rpc-secret|#rpc-secret|g' /opt/aria2/.aria2/aria2.conf && \
        sed -i 's|/root/.aria2|/opt/aria2/.aria2|g' /opt/aria2/.aria2/aria2.conf && \
        sed -i 's|/root/.aria2|/opt/aria2/.aria2|g' /opt/aria2/.aria2/script.conf && \
        sed -i 's|/root|/opt/aria2|g' /opt/aria2/.aria2/aria2.conf && \
        sed -i 's|/root|/opt/aria2|g' /opt/aria2/.aria2/script.conf && \
        mkdir -p /opt/service/stop/aria2/log && \
        echo '#!/bin/sh' > /opt/service/stop/aria2/run && \
        echo 'exec 2>&1' >> /opt/service/stop/aria2/run && \
        echo 'exec aria2c --enable-rpc --rpc-allow-origin-all --conf-path=/opt/aria2/.aria2/aria2.conf' >> /opt/service/stop/aria2/run && \
        echo '#!/bin/sh' > /opt/service/stop/aria2/log/run && \
        echo 'mkdir -p /opt/openlist/data/log/aria2 2>/dev/null' >> /opt/service/stop/aria2/log/run && \
        echo 'exec svlogd /opt/openlist/data/log/aria2' >> /opt/service/stop/aria2/log/run && \
        chmod +x /opt/service/stop/aria2/run /opt/service/stop/aria2/log/run && \
        touch /opt/aria2/.aria2/aria2.session && \
        /opt/aria2/.aria2/tracker.sh ; \
    rm -rf /var/cache/apk/*
    
RUN mkdir -p /opt/service/start/openlist && \
    echo '#!/bin/sh' > /opt/service/start/openlist/run && \
    echo 'exec 2>&1' >> /opt/service/start/openlist/run && \
    echo 'cd /opt/openlist' >> /opt/service/start/openlist/run && \
    echo 'exec ./openlist server --no-prefix' >> /opt/service/start/openlist/run && \
    chmod +x /opt/service/start/openlist/run