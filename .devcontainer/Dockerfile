# syntax=docker/dockerfile:1
FROM --platform=linux/amd64 ubuntu:22.04

ARG BLENDER_VERSION=4.2.3
ARG BLENDER_TAR=blender-${BLENDER_VERSION}-linux-x64.tar.xz
ARG BLENDER_URL=https://download.blender.org/release/Blender4.2/${BLENDER_TAR}
ARG BLENDER_SHA256=3a64efd1982465395abab4259b4091d5c8c56054c7267e9633e4f702a71ea3f4

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update 
RUN apt-get install -y --no-install-recommends \
    ca-certificates curl xz-utils \
    python3 python3-pip \
    git \
    libx11-6 libxext6 libxrender1 libxi6 libxfixes3 libxkbcommon0 \
    libgl1 libglib2.0-0 libsm6 libice6 \
    && rm -rf /var/lib/apt/lists/*

# Blender
RUN curl -fsSL "${BLENDER_URL}" -o /tmp/blender.tar.xz \
 && echo "${BLENDER_SHA256}  /tmp/blender.tar.xz" | sha256sum -c - \
 && mkdir -p /opt/blender \
 && tar -xJf /tmp/blender.tar.xz -C /opt/blender --strip-components=1 \
 && rm /tmp/blender.tar.xz

ENV PATH="/opt/blender:${PATH}"

WORKDIR /work