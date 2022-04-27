ARG BUILD_VERSION
FROM jonaswinkler/paperless-ng:$BUILD_VERSION

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y redis && \
    apt-get clean
COPY ./redis-supervisord.conf /tmp/redis-supervisord.conf
ENV PAPERLESS_REDIS=redis://localhost:6379
RUN cat /tmp/redis-supervisord.conf >> /etc/supervisord.conf