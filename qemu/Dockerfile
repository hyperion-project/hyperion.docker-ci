ARG QEMU_VERSION=11.1.1

FROM --platform=$BUILDPLATFORM debian:bookworm-slim AS passthrough
RUN mkdir -p /out && \
    printf '#!/bin/sh\nexec "$@"\n' > /out/qemu-static && \
    chmod +x /out/qemu-static

FROM --platform=$BUILDPLATFORM ubuntu:24.04 AS qemu-build

ARG QEMU_VERSION
ARG TARGETARCH
ARG TARGETVARIANT

COPY --from=passthrough /out/qemu-static /out/qemu-static
WORKDIR /src
COPY patches/ /patches/

RUN if [ "${TARGETARCH}" != "amd64" ]; then \
     apt-get update && apt-get install -y --no-install-recommends \
          build-essential \
          ninja-build \
          pkg-config \
          patch \
          curl \
          ca-certificates \
          xz-utils \
          file \
          python3 \
          python3-venv \
          python3-pip \
          libglib2.0-dev \
          libpcre2-dev \
          zlib1g-dev && \
     case "${TARGETARCH}${TARGETVARIANT}" in \
          armv6|armv7) \
               TARGETS="arm-linux-user" \
               ;; \
          arm64) \
               TARGETS="aarch64-linux-user" \
               ;; \
          *) exit 1 ;; \
     esac && \
     curl -fsSLO "https://download.qemu.org/qemu-${QEMU_VERSION}.tar.xz" && \
     tar -xf "qemu-${QEMU_VERSION}.tar.xz" && \
     rm "qemu-${QEMU_VERSION}.tar.xz" && \
     cd "qemu-${QEMU_VERSION}" && \
     if ls /patches/*.patch >/dev/null 2>&1; then \
          for patch in /patches/*.patch; do \
               patch -p1 --no-backup-if-mismatch < "$patch" || exit 1; \
          done; \
     fi && \
     mkdir /build && cd /build && \
     "/src/qemu-${QEMU_VERSION}/configure" \
          --static \
          --target-list=${TARGETS} \
          --disable-system \
          --disable-tools \
          --disable-docs \
          --disable-guest-agent \
          --disable-plugins \
          --disable-download && \
     ninja -j"$(nproc)" && \
     mkdir -p /out && \
     if [ -x "/build/${TARGETS}" ]; then \
          ${CROSS_PREFIX}strip -o "/out/${TARGETS}" "/build/${TARGETS}"; \
          cp "/out/${TARGETS}" "/out/qemu-static"; \
     fi && \
     test -n "$(ls -A /out)"; \
fi

FROM scratch AS qemu
COPY --from=qemu-build /out/ /qemu/