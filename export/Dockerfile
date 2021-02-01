
FROM crosstool-ng AS build

ARG TOOLCHAIN=x86_64-unknown-linux-gnu
ARG THREADS=4

RUN --mount=type=cache,target=/root/src \
    set -x; \
    ct-ng "${TOOLCHAIN}" && \
    ct-ng "build.${THREADS}"

FROM debian:10

ARG TOOLCHAIN=x86_64-unknown-linux-gnu

RUN  --mount=type=cache,target=/var/cache/apt \
    apt-get update && \
    apt-get install -y hardlink
    
COPY --from=build \
    /root/x-tools/${TOOLCHAIN}/ /usr/local/
    
RUN  hardlink /usr/local/