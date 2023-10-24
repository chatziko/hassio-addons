ARG BUILD_FROM
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install porcupine
WORKDIR /usr/src
ENV WYOMING_PORCUPINE_VERSION=4f40a961cc16c54108f56df961a9311921860e24
ENV PIP_BREAK_SYSTEM_PACKAGES=1

RUN \
    apt-get update \
    && apt-get install -y --no-install-recommends \
        netcat-traditional \
        python3 \
        git \
        python3-pip \
    \
    && pip3 install --no-cache-dir -U \
        setuptools \
        wheel \
    && pip3 install --no-cache-dir \
        --extra-index-url https://www.piwheels.org/simple \
        "wyoming-porcupine@git+https://github.com/chatziko/wyoming-porcupine@4f40a961cc16c54108f56df961a9311921860e24" \
    \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /
COPY rootfs /

HEALTHCHECK --start-period=10m \
    CMD echo '{ "type": "describe" }' \
        | nc -w 1 localhost 10400 \
        | grep -iq "porcupine" \
        || exit 1
