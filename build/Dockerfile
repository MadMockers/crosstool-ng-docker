# syntax = docker/dockerfile:1.2.1

FROM debian:10

ARG VERSION=1.24.0
ARG THREADS=4

RUN --mount=type=cache,target=/var/cache/apt/ \
    set -x; \
    apt-get update && \
    apt-get install -y wget \
                       python3 \
                       bzip2 \
                       gpg \
                       build-essential \
                       gperf \
                       bison \
                       flex \
                       texinfo \
                       help2man \
                       diffutils \
                       autoconf \
                       gawk \
                       libncurses-dev \
                       unzip \
                       libtool-bin \
                       python3-dev

RUN --mount=type=cache,target=/tmp/downloads/ \
    --mount=type=bind,target=/ctxt/ \
    set -x; \
    cd /tmp/downloads && \
    wget http://crosstool-ng.org/download/crosstool-ng/crosstool-ng-${VERSION}.tar.bz2 && \
    wget http://crosstool-ng.org/download/crosstool-ng/crosstool-ng-${VERSION}.tar.bz2.sig && \
    gpg --import /ctxt/keys.gpg && \
    gpg --verify crosstool-ng-${VERSION}.tar.bz2.sig

RUN --mount=type=cache,target=/tmp/build/ \
    --mount=type=cache,target=/tmp/downloads/ \
    --mount=type=bind,target=/ctxt/ \
    set -x && \
    cd /tmp/build/ && \
    tar xf /tmp/downloads/crosstool-ng-${VERSION}.tar.bz2 && \
    cd crosstool-ng-${VERSION} && \
    if [ -e /ctxt/${VERSION}/fixup.sh ]; then /bin/bash /ctxt/${VERSION}/fixup.sh; fi && \
    autoconf && \
    ./configure && \
    make -j${THREADS} && \
    make install

WORKDIR /opt/
ENV CT_ALLOW_BUILD_AS_ROOT_SURE=1