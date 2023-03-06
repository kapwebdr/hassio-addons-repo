ARG BUILD_FROM
FROM ${BUILD_FROM}

ENV LANG C.UTF-8

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base system
ARG BUILD_ARCH=amd64

RUN \
    apt-get update \
    && apt-get install -y --no-install-recommends procps \
    && ARCH="${BUILD_ARCH}" \
    && if [ "${BUILD_ARCH}" = "aarch64" ]; then ARCH="arm64"; fi \
    && if [ "${BUILD_ARCH}" = "armv7" ]; then ARCH="armhf"; fi \
    \
    && INFLUXDB="2.6.1" \
    && curl -J -L -o /tmp/influxdb.deb \
        "https://dl.influxdata.com/influxdb/releases/influxdb2-${INFLUXDB}-${ARCH}.deb" \
    \
    && dpkg --install /tmp/influxdb.deb \
    && curl -J -L -o /tmp/influxdb-client.deb \
        "https://dl.influxdata.com/influxdb/releases/influxdb2-client-${INFLUXDB}-${ARCH}.deb" \
    \
    && dpkg --install /tmp/influxdb-client.deb \
    \
    && rm -fr \
        /tmp/* \
        /var/{cache,log}/* \
        /var/lib/apt/lists/*

COPY --chown=root run /etc/services.d/influxdb2/
COPY --chown=root finish /etc/services.d/influxdb2/
RUN chmod a+x /etc/services.d/influxdb2/*

# S6-Overlay
ENTRYPOINT ["/init"]