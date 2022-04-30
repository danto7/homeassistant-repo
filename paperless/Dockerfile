ARG BUILD_FROM
FROM ${BUILD_FROM}

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y redis nginx && \
    apt-get clean

# patch django config
RUN echo "USE-X-FORWARDED-HOST=True" >> /usr/src/paperless/src/paperless/seetings.py

COPY nginx.conf /etc/nginx/nginx.conf
RUN nginx -t

COPY ./redis-supervisord.conf /tmp/redis-supervisord.conf
RUN cat /tmp/redis-supervisord.conf >> /etc/supervisord.conf

ENV PAPERLESS_REDIS=redis://localhost:6379
