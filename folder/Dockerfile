ARG BASE=latest

FROM golang:1-bookworm AS build

ARG WALG_RELEASE=master

RUN apt-get update && apt-get install -qqy --no-install-recommends --no-install-suggests cmake gnutls-bin liblzo2-dev libsodium23 libsodium-dev && \
    cd /go/src/ && \
    git clone -b $WALG_RELEASE --recurse-submodules https://github.com/wal-g/wal-g.git && \
    cd wal-g && \
    GOBIN=/go/bin go mod tidy && \
    GOBIN=/go/bin USE_LIBSODIUM=1 USE_LZO=1 make install_and_build_pg


FROM postgres:$BASE


RUN export DEBIAN_FRONTEND=noninteractive && \
    apt-get update && \
    apt-get install -qqy curl ca-certificates libsodium23 vim

COPY wal-g /wal-g
