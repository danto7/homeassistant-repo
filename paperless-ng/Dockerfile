ARG BUILD_FROM

FROM alpine AS downloader
ADD https://github.com/jonaswinkler/paperless-ng/archive/refs/tags/ng-1.5.0.zip ./artifact.zip
RUN mkdir /build && \
    unzip ./artifact.zip -d /build

FROM node:14-alpine AS frontend
RUN mkdir /build
WORKDIR /build
COPY --from=downloader /build/paperless* .
RUN sh ./compile-frontend.sh

FROM $BUILD_FROM
RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y build-essential python3-setuptools python3-wheel && \
    apt-get install -y --no-install-recommends gettext liblept5 && \
    mkdir /build
WORKDIR /build
COPY --from=downloader /build/paperless* .
RUN pip install --no-cache-dir -r requirements.txt && \
    mkdir -p dist && \
    cp .dockerignore .env Dockerfile Pipfile Pipfile.lock LICENSE README.md requirements.txt dist/ && \
    cp paperless.conf.example dist/paperless.conf && \
    cp gunicorn.conf.py dist/gunicorn.conf.py && \
    cp src/ dist/src -r && \
    cd dist/src && \
    python manage.py compilemessages && \
    python manage.py collectstatic --no-input && \
    mv /build/dist /opt/paperless && \
    rm -rf /build

LABEL \
  io.hass.version="VERSION" \
  io.hass.type="addon" \
  io.hass.arch="armv7|aarch64|amd64"

WORKDIR /opt/paperless
RUN export MAKEFLAGS="-j $(nproc)" && \
    adduser --system --home /opt/paperless --group paperless && \
    chown -R paperless:paperless /opt/paperless
USER paperless
