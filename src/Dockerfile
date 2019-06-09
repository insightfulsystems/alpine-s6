ARG BASE
FROM ${BASE}
ARG ARCH

RUN apk add --no-cache --purge -uU \
        curl \
 && VERSION=$(curl -SL https://api.github.com/repos/just-containers/s6-overlay/releases/latest | awk '/tag_name/{print $4;exit}' FS='[""]' | sed -e 's_v__') \
 && echo "using s6 version: $VERSION-$ARCH" \
 && curl -o /tmp/s6-overlay-${ARCH}.tar.gz -jkSL https://github.com/just-containers/s6-overlay/releases/download/v${VERSION}/s6-overlay-${ARCH}.tar.gz \
 && tar xzf /tmp/s6-overlay-${ARCH}.tar.gz -C / \
 && apk del --purge curl \
 && useradd -u 911 -U -d /config -s /bin/false abc \
 && usermod -G users abc \
 && mkdir -p /app /data /config /defaults \
 && rm -rf /var/cache/apk/* /tmp/*
