# syntax=docker/dockerfile:1

# Blender download stage
FROM --platform=linux/amd64 mcr.microsoft.com/devcontainers/base:latest AS blender-builder

ARG BLENDER_VERSION=4.2.3
ARG BLENDER_TAR=blender-${BLENDER_VERSION}-linux-x64.tar.xz
ARG BLENDER_URL=https://download.blender.org/release/Blender4.2/${BLENDER_TAR}
# Official checksum list for this Blender version (contains checksums for multiple artifacts).
ARG BLENDER_SHA256_URL=https://download.blender.org/release/Blender4.2/blender-${BLENDER_VERSION}.sha256

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    xz-utils \
    && rm -rf /var/lib/apt/lists/*

RUN curl -fsSL "${BLENDER_URL}" -o "/tmp/${BLENDER_TAR}" && \
    curl -fsSL "${BLENDER_SHA256_URL}" -o "/tmp/blender-${BLENDER_VERSION}.sha256" && \
    cd /tmp && sha256sum -c "blender-${BLENDER_VERSION}.sha256" --ignore-missing && \
    mkdir -p /opt/blender && \
    tar -xJf "/tmp/${BLENDER_TAR}" -C /opt/blender --strip-components=1 && \
    rm "/tmp/${BLENDER_TAR}" "/tmp/blender-${BLENDER_VERSION}.sha256"

# Final stage - Microsoft Python image
# mcr.microsoft.com/devcontainers/python https://hub.docker.com/r/microsoft/devcontainers-python
# Blender 4.2.3 requires Python 3.11.*
FROM --platform=linux/amd64 mcr.microsoft.com/devcontainers/python:3.11

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends \
    libx11-6 \
    libxext6 \
    libxrender1 \
    libxfixes3 \
    libxi6 \
    libxkbcommon0 \
    libglib2.0-0 \
    libsm6 \
    libice6 \
    libxxf86vm1 \
    libgl1 \
    && rm -rf /var/lib/apt/lists/*

# Copy Blender binary from previous stage
COPY --from=blender-builder /opt/blender /opt/blender

ENV PATH="/opt/blender:${PATH}"

WORKDIR /work
