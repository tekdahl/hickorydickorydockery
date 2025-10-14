# syntax = docker/dockerfile:1
# start from golang alpine image
FROM golang:1.25.2-alpine3.22 AS builder

# Install Neocities
RUN apk add --no-cache ruby gcc musl-dev
RUN gem install neocities

# set dependency versions to download
ARG DART_SASS_VERSION=1.93.2
ARG HUGO_VERSION=0.151.0
ARG NODE_VERSION=22.18.0

# download dependencies
ADD https://github.com/sass/dart-sass/releases/download/${DART_SASS_VERSION}/dart-sass-${DART_SASS_VERSION}-linux-x64.tar.gz /dart-sass.tar.gz
ADD https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.tar.gz /hugo.tar.gz
ADD https://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-x64.tar.xz /node.tar.xz

ENV HOME=/root

# install
RUN mkdir -p $HOME/.local \
    && tar -C "${HOME}/.local" -xf "/dart-sass.tar.gz"

RUN mkdir "${HOME}/.local/hugo" \
    && tar -C "${HOME}/.local/hugo" -xf "/hugo.tar.gz"

RUN tar -C "${HOME}/.local/" -xf "/node.tar.xz"

# restart build since gcc and musl-dev should only be necessary to compile
FROM golang:1.25.2-alpine3.22

# Install utilities
RUN apk add --no-cache brotli xz zstd git ruby

ENV HOME=/root

# copy dependencies from $HOME/.local
WORKDIR $HOME/.local/
COPY --from=builder /root/.local/ .

# copy neocities gem install
WORKDIR /usr/lib/ruby/
COPY --from=builder /usr/lib/ruby/ .

# copy neocities gem executable
WORKDIR /usr/bin/
COPY --from=builder /usr/bin/neocities .

# set PATH
ENV DART_SASS_VERSION=1.93.2
ENV HUGO_VERSION=0.151.0
ENV NODE_VERSION=22.18.0
ENV PATH="$HOME/.local/dart-sass:$HOME/.local/hugo:$HOME/.local/node-v${NODE_VERSION}-linux-x64/bin:$PATH"
