ARG BUILD_VERSION
FROM jonaswinkler/paperless-ng:$BUILD_VERSION
LABEL \
  io.hass.version="VERSION" \
  io.hass.type="addon" \
  io.hass.arch="armv7|aarch64|amd64"

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y redis && \
    apt-get clean
COPY ./redis-supervisord.conf /tmp/redis-supervisord.conf
ENV PAPERLESS_REDIS=redis://localhost:6379
RUN cat /tmp/redis-supervisord.conf >> /etc/supervisord.conf