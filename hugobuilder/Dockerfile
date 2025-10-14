# syntax = docker/dockerfile:1
# start from golang alpine image
FROM golang:1.25.2-alpine3.22 AS builder

# Install Neocities
RUN apk add --no-cache ruby gcc musl-dev
RUN gem install neocities

# restart build since gcc and musl-dev should only be necessary to compile
FROM golang:1.25.2-alpine3.22

# set env vars
ENV DART_SASS_VERSION="1.92.1-r0"
ENV HUGO_VERSION="0.151.0-r1"
ENV NODE_VERSION="22.16.0-r2"

# Install utilities
RUN apk add --no-cache brotli xz zstd git ruby nodejs=$NODE_VERSION
RUN apk add --no-cache dart-sass=$DART_SASS_VERSION  --repository=http://dl-cdn.alpinelinux.org/alpine/edge/testing/
RUN apk add --no-cache hugo=$HUGO_VERSION  --repository=http://dl-cdn.alpinelinux.org/alpine/edge/community/

# copy neocities gem install
WORKDIR /usr/lib/ruby/
COPY --from=builder /usr/lib/ruby/ .

# copy neocities gem executable
WORKDIR /usr/bin/
COPY --from=builder /usr/bin/neocities .
