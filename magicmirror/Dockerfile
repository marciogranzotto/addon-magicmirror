ARG BUILD_FROM=hassioaddons/base:8.0.1
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set ENV
ENV MM_VERSION="v2.12.0" \
    MMM_HASS='7d0063b0bdfffc9b8d4c751fb31dc0ea0f4e39e4'

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Sets working directory
WORKDIR /opt

# Setup base
# hadolint ignore=DL3003
RUN \
  apk add --no-cache \
      git=2.26.2-r0 \
      lua-resty-http=0.15-r0 \
      nginx-mod-http-lua=1.18.0-r0 \
      nginx=1.18.0-r0 \
      nodejs=12.18.3-r0 \
      npm=12.18.3-r0 \
  \
  && npm config set unsafe-perm true \
  \
  && npm install -g modclean@3.0.0-beta.1 \
  \
  && git clone --branch "${MM_VERSION}" --depth=1 \
    https://github.com/MichMich/MagicMirror.git /opt \
  \
  && npm install \
    --no-optional \
    --only=production \
  \
  && git clone \
    https://github.com/aserramonner/MMM-HASS.git /opt/modules/MMM-HASS \
  \
  && cd /opt/modules/MMM-HASS \
  && npm install \
    --no-optional \
    --only=production \
  \
  && modclean \
    --path /opt \
    --no-progress \
    --keep-empty \
    --run \
  \
  && npm uninstall -g modclean \
  && npm cache clear --force \
  \
  && rm -f -r \
    /tmp/* \
    /opt/.git \
    /opt/modules/MMM-HASS/.git \
    /root/.electron

# Copy root filesystem
COPY rootfs /

# Build arugments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="MagicMirror²" \
    io.hass.description="MagicMirror² is an open source modular smart mirror platform." \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Joakim Sørensen @ludeeus <ludeeus@gmail.com>" \
    org.label-schema.description="MagicMirror² is an open source modular smart mirror platform." \
    org.label-schema.build-date=${BUILD_DATE} \
    org.label-schema.name="MagicMirror²" \
    org.label-schema.schema-version="1.0" \
    org.label-schema.url="https://community.home-assistant.io/t/community-hass-io-add-on-magicmirror/XXXX" \
    org.label-schema.usage="https://github.com/hassio-addons/addon-magicmirror/tree/master/README.md" \
    org.label-schema.vcs-ref=${BUILD_REF} \
    org.label-schema.vcs-url="https://github.com/hassio-addons/addon-magicmirror" \
    org.label-schema.vendor="Home Assistant Community Add-ons"
