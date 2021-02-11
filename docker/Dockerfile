FROM nvidia/cuda:11.1.1-cudnn8-devel-ubuntu20.04

ENV DEBIAN_FRONTEND noninteractive
ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES video,compute,utility

# Install required Packages

RUN set -xe && \
    apt-get update && \
    apt-get install -y \
            git sudo wget \
            build-essential \
            ffmpeg \
            gdebi \
            libopencv-dev \
            cmake

# Build OpenPose

WORKDIR /usr/local
RUN set -xe && \
    git clone https://github.com/CMU-Perceptual-Computing-Lab/openpose && \
    cd /usr/local/openpose && \
    bash ./scripts/ubuntu/install_deps.sh

RUN mkdir -p /usr/local/openpose/build && \
    cd /usr/local/openpose/build && \
    cmake .. && \
    make -j$(nproc) && \
    make install

# Cleanup

WORKDIR /usr/local
RUN set -xe && \
    apt-get clean && \
    rm -rf /tmp/cmake.tar.gz && \
    ln -s /usr/local/openpose/build/examples/openpose/openpose.bin /usr/local/bin/openpose.bin

