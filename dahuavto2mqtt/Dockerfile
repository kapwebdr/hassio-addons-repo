ARG BUILD_FROM
FROM ${BUILD_FROM}

ENV LANG C.UTF-8

ARG DAHUA_BRANCH="master"

WORKDIR /app

RUN apk update && apk upgrade
RUN apk add --no-cache python3 py3-pip py3-setuptools git
RUN pip3 install --upgrade pip --no-cache-dir paho-mqtt requests

RUN git clone https://gitlab.com/elad.bar/DahuaVTO2MQTT.git --branch ${DAHUA_BRANCH} --depth 1 /app

COPY --chown=root run /etc/services.d/dahuavto/
COPY --chown=root finish /etc/services.d/dahuavto/
RUN chmod a+x /etc/services.d/dahuavto/*

# S6-Overlay
ENTRYPOINT ["/init"]