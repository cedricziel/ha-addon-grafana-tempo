# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM=ghcr.io/home-assistant/aarch64-base:3.15

# Loki
FROM --platform=linux/amd64 grafana/tempo:1.5.0 as tempo-amd64
FROM --platform=linux/arm64 grafana/tempo:1.5.0 as tempo-arm64

FROM scratch as container

COPY --from=tempo-amd64 /tempo /amd64/
COPY --from=tempo-arm64 /tempo /aarch64/

FROM $BUILD_FROM

# Execute during the build of the image
ARG TEMPIO_VERSION BUILD_ARCH
RUN \
    curl -sSLf -o /usr/bin/tempio \
    "https://github.com/home-assistant/tempio/releases/download/${TEMPIO_VERSION}/tempio_${BUILD_ARCH}"

COPY --from=container /${BUILD_ARCH}/tempo /tempo

# Copy root filesystem
COPY rootfs /
