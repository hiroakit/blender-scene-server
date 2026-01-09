# syntax=docker/dockerfile:1

# Blender download stage
FROM --platform=linux/amd64 debian:bookworm-slim AS blender-builder

ARG BLENDER_VERSION=4.2.3
ARG BLENDER_TAR=blender-${BLENDER_VERSION}-linux-x64.tar.xz
ARG BLENDER_URL=https://download.blender.org/release/Blender4.2/${BLENDER_TAR}

RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates curl xz-utils \
    && rm -rf /var/lib/apt/lists/*

RUN curl -L "${BLENDER_URL}" -o /tmp/blender.tar.xz && \
    mkdir -p /opt/blender && \
    tar -xJf /tmp/blender.tar.xz -C /opt/blender --strip-components=1 && \
    rm /tmp/blender.tar.xz

# Final stage - Microsoft Python image
FROM --platform=linux/amd64 mcr.microsoft.com/python:3.11-slim

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    git \
    libx11-6 \
    libxext6 \
    libxrender1 \
    libxfixes3 \
    libglib2.0-0 \
    libsm6 \
    libice6 \
    && rm -rf /var/lib/apt/lists/*

# Copy Blender binary from previous stage
COPY --from=blender-builder /opt/blender /opt/blender

ENV PATH="/opt/blender:${PATH}"

WORKDIR /work
