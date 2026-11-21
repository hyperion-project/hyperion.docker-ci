ARG OPENSSL_VERSION=3.6.0
ARG CMAKE_VERSION=3.28.3

# ------------------------------------------
# Stage 1: Prebuilt binaries (amd64 / arm64)
# ------------------------------------------

FROM buildpack-deps:bookworm-curl AS download

ARG CMAKE_VERSION
ARG TARGETARCH

RUN case "${TARGETARCH}" in \
        amd64) A=x86_64 ;; \
        arm64) A=aarch64 ;; \
        *) exit 1 ;; \
    esac && \
    mkdir /out && cd /out && \
    curl -fsSLO "https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERSION}/cmake-${CMAKE_VERSION}-linux-${A}.sh"

# ------------------------------------------
# Stage 2: ARM base images
# ------------------------------------------

# armv6 (Raspbian Bookworm) (glibc 2.36)
FROM --platform=$BUILDPLATFORM debian:bookworm-slim AS raspbian-base

RUN --security=insecure \
    apt-get update && apt-get install -y --no-install-recommends ca-certificates curl gpgv xz-utils debootstrap qemu-user-static binfmt-support && \
    curl -fsSL "https://archive.raspbian.org/raspbian/pool/main/r/raspbian-archive-keyring/raspbian-archive-keyring_20120528.4_all.deb" -o /tmp/r.deb && \
    dpkg-deb -x /tmp/r.deb /tmp/r-key && \
    debootstrap --keyring=/tmp/r-key/usr/share/keyrings/raspbian-archive-keyring.gpg --arch="armhf" --variant="minbase" "bookworm" /tmp/rootfs "http://archive.raspbian.org/raspbian/" && \
    install -D -m644 /tmp/r-key/usr/share/keyrings/raspbian-archive-keyring.gpg /tmp/rootfs/etc/apt/trusted.gpg.d/raspbian-archive-keyring.gpg && \
    rm -rf /tmp/r.deb /tmp/r-key

FROM scratch AS base-armv6
COPY --from=raspbian-base /tmp/rootfs /

# armv7 (Ubuntu 22.04) (glibc 2.35)
FROM ubuntu:22.04 AS base-armv7

# ------------------------------------------
# Stage 3: OpenSSL (arm only)
# ------------------------------------------
FROM base-${TARGETARCH}${TARGETVARIANT} AS openssl-build

ARG OPENSSL_VERSION

RUN apt-get update && apt-get install -y --no-install-recommends build-essential ca-certificates curl perl

# OpenSSL
RUN case "${TARGETVARIANT}" in \
        v6) MARCH="-march=armv6" ;; \
        v7) MARCH="-march=armv7-a" ;; \
        *)  MARCH="" ;; \
    esac && \
    mkdir -p /opt/openssl/src && cd /opt/openssl/src && \
    curl -fsSLO "https://github.com/openssl/openssl/releases/download/openssl-${OPENSSL_VERSION}/openssl-${OPENSSL_VERSION}.tar.gz" && \
    tar -xzf "openssl-${OPENSSL_VERSION}.tar.gz" && \
    cd "openssl-${OPENSSL_VERSION}" && \
    ./Configure linux-armv4 ${MARCH} --prefix=/opt/openssl --libdir=lib no-asm no-shared no-tests && \
    make -j"$(nproc)" install_dev && \
    cd / && rm -rf /opt/openssl/src

# ------------------------------------------
# Stage 4: CMake (arm only)
# ------------------------------------------
FROM openssl-build AS cmake-build

ARG CMAKE_VERSION
ARG TARGETARCH
ARG TARGETVARIANT

RUN apt-get update && apt-get install -y --no-install-recommends ninja-build

WORKDIR /src
RUN curl -fsSLO "https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERSION}/cmake-${CMAKE_VERSION}.tar.gz" && \
    tar -xzf "cmake-${CMAKE_VERSION}.tar.gz" --strip-components 1 && \
    mkdir /build && cd /build && \
    case "${TARGETVARIANT}" in \
        v6|v7) EXTRA_C_FLAGS='-DCMAKE_C_STANDARD_LIBRARIES=-latomic' ;; \
        *)     EXTRA_C_FLAGS='' ;; \
    esac && \
    /src/bootstrap --parallel="$(nproc)" --generator=Ninja -- \
        -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_C_STANDARD=11 \
        -DCMAKE_CXX_STANDARD=14 \
        -DCMAKE_EXE_LINKER_FLAGS="-static-libstdc++ -static-libgcc" \
        ${EXTRA_C_FLAGS} \
        -DCMAKE_CXX_STANDARD_LIBRARIES="-Wl,-Bstatic -latomic -Wl,-Bdynamic" \
        -DCMAKE_USE_OPENSSL=ON \
        -DOPENSSL_CRYPTO_LIBRARY=/opt/openssl/lib/libcrypto.a \
        -DOPENSSL_INCLUDE_DIR=/opt/openssl/include \
        -DOPENSSL_SSL_LIBRARY=/opt/openssl/lib/libssl.a \
        -DBUILD_CursesDialog=OFF \
        -DBUILD_QtDialog=OFF \
        -DBUILD_TESTING=OFF \
        -DCMake_TEST_BOOTSTRAP=FALSE \
        -DCPACK_SYSTEM_NAME="linux-${TARGETARCH}${TARGETVARIANT}l" && \
    ninja -j"$(nproc)" && \
    bin/cpack -G STGZ && \
    mkdir /out && \
    mv cmake-*-linux-"${TARGETARCH}${TARGETVARIANT}l".* /out/

# ------------------------------------------
# Stage 5: Final image
# ------------------------------------------

FROM scratch AS cmake-arm
COPY --from=cmake-build /out/ /cmake/

FROM scratch AS cmake-amd64
COPY --from=download /out/ /cmake/

FROM scratch AS cmake-arm64
COPY --from=download /out/ /cmake/

FROM cmake-${TARGETARCH} AS cmake