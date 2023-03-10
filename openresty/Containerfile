ARG RESTY_IMAGE_BASE="alpine"
ARG RESTY_IMAGE_TAG="3.15"

FROM ${RESTY_IMAGE_BASE}:${RESTY_IMAGE_TAG}

LABEL maintainer="Pedro Maia Martins de Sousa < brpedromaia@gmail.com >"

# Docker Build Arguments
ARG RESTY_IMAGE_BASE="alpine"
ARG RESTY_IMAGE_TAG="3.15"

ARG RESTY_APK_KEY_URL="https://openresty.org/package/admin@openresty.com-5ea678a6.rsa.pub"
ARG RESTY_APK_REPO_URL="https://openresty.org/package/alpine/v${RESTY_IMAGE_TAG}/main"
ARG RESTY_APK_VERSION="=1.21.4.1-r0"

RUN wget -O "/etc/apk/keys/$(basename ${RESTY_APK_KEY_URL})" "${RESTY_APK_KEY_URL}" \
    && echo "${RESTY_APK_REPO_URL}" >> /etc/apk/repositories \
    && apk update \
    && apk add "openresty${RESTY_APK_VERSION}" \
    && mkdir -p /var/run/openresty \
    && ln -sf /dev/stdout /usr/local/openresty/nginx/logs/access.log \
    && ln -sf /dev/stderr /usr/local/openresty/nginx/logs/error.log \
    && apk update \
    && apk add --no-cache bash curl jq nmap-ncat yq openssl \
    && rm -rf /var/cache/apk/* /tmp/* /sbin/halt /sbin/poweroff /sbin/reboot

SHELL ["/bin/bash", "-c"]

# Add additional binaries into PATH for convenience
ENV PATH=$PATH:/usr/local/openresty/luajit/bin:/usr/local/openresty/nginx/sbin:/usr/local/openresty/bin

# Copy nginx configuration files
COPY nginx/ /usr/local/openresty/nginx/
COPY bin/ /usr/local/bin/

ENTRYPOINT ["/usr/local/bin/container-entrypoint" ]

# Use SIGQUIT instead of default SIGTERM to cleanly drain requests
# See https://github.com/openresty/docker-openresty/blob/master/README.md#tips--pitfalls
STOPSIGNAL SIGQUIT