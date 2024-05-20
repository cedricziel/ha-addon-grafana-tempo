# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base:3.15

# Tempo
# hadolint ignore=DL3029
FROM --platform=linux/amd64 grafana/tempo:2.4.2 as tempo-amd64
# hadolint ignore=DL3029
FROM --platform=linux/arm64 grafana/tempo:2.4.2 as tempo-arm64

FROM scratch as container

COPY --from=tempo-amd64 /tempo /amd64/
COPY --from=tempo-arm64 /tempo /aarch64/

# hadolint ignore=DL3006
FROM $BUILD_FROM

# Setup base system
ARG BUILD_ARCH=amd64

COPY --from=container /${BUILD_ARCH}/tempo /tempo

# Copy root filesystem
COPY rootfs /
