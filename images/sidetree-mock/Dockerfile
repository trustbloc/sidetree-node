#
# Copyright SecureKey Technologies Inc. All Rights Reserved.
#
# SPDX-License-Identifier: Apache-2.0
#

ARG GO_VER
ARG ALPINE_VER

FROM golang:${GO_VER}-alpine${ALPINE_VER} as golang
RUN apk add --no-cache \
	gcc \
	musl-dev \
	git \
	libtool \
	bash \
	make;
ADD . $GOPATH/src/github.com/trustbloc/sidetree-mock
WORKDIR $GOPATH/src/github.com/trustbloc/sidetree-mock
ENV EXECUTABLES go git

FROM golang as sidetree-mock
ARG GO_TAGS
ARG GOPROXY
RUN GO_TAGS=${GO_TAGS} GOPROXY=${GOPROXY} make sidetree-mock


FROM alpine:${ALPINE_VER} as base
LABEL org.opencontainers.image.source https://github.com/trustbloc/sidetree-mock

COPY --from=sidetree-mock /go/src/github.com/trustbloc/sidetree-mock/.build/bin/sidetree-mock /usr/local/bin
CMD ["sidetree-mock"]
