ARG BASE
FROM ${BASE}
ARG ARCH

RUN apk add --no-cache --purge -uU \
        curl \
		gnupg \
 && VERSION=$(curl -SL https://api.github.com/repos/just-containers/s6-overlay/releases/latest | awk '/tag_name/{print $4;exit}' FS='[""]' | sed -e 's_v__') \
 && echo "using s6 version: $VERSION-$ARCH" \
 && curl -o /tmp/s6-overlay-${ARCH}.tar.gz -jkSL https://github.com/just-containers/s6-overlay/releases/download/v${VERSION}/s6-overlay-${ARCH}.tar.gz \
 && curl -o /tmp/s6-overlay-${ARCH}.tar.gz.sig -jkSL https://github.com/just-containers/s6-overlay/releases/download/v${VERSION}/s6-overlay-${ARCH}.tar.gz.sig \
 && curl https://keybase.io/justcontainers/key.asc | gpg --import \
 && gpg --verify /tmp/s6-overlay-${ARCH}.tar.gz.sig /tmp/s6-overlay-${ARCH}.tar.gz \
 && tar xzf /tmp/s6-overlay-${ARCH}.tar.gz -C / \
 && apk del --purge \
    curl \
	gnupg \
 && adduser \
		--disabled-password \
		--gecos "" \ 
		--home /config \
		--ingroup users \
		--no-create-home \
		--shell /bin/false \
		--uid 911 \
		abc \
 && mkdir -p /app /config /data /defaults \
 && rm -rf /var/cache/apk/* /tmp/*

COPY rootfs /

ENTRYPOINT [ "/init" ]

ARG VCS_REF
ARG VCS_URL
ARG BUILD_DATE
LABEL org.label-schema.vcs-ref=${VCS_REF} \
      org.label-schema.vcs-url=${VCS_URL} \
      org.label-schema.build-date=${BUILD_DATE}