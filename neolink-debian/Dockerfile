# SPDX-License-Identifier: AGPL-3.0-only
FROM docker.io/rust:buster AS build

ENV DEBIAN_FRONTEND=noninteractive
RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
    build-essential \
    musl-dev gcc \
    libgstrtspserver-1.0-dev \
    libgstreamer1.0-dev \
    libgtk2.0-dev \
    libglib2.0-dev; \
    apt-get clean -y; \
    rm -rf /var/lib/apt/lists/* /var/cache/apt/*

# Use static linking to work around https://github.com/rust-lang/rust/pull/58575
ENV RUSTFLAGS='-C target-feature=-crt-static'

# Clone respository for all the goodies
WORKDIR /usr/local/src
RUN git clone https://github.com/thirtythreeforty/neolink

# Move to source
WORKDIR /usr/local/src/neolink

# Build the main program
COPY . /usr/local/src/neolink
RUN cargo build --release

# Create the release container. Match the base OS used to build
FROM debian:buster-slim

ENV DEBIAN_FRONTEND=noninteractive
RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
    musl \
    libgstrtspserver-1.0-0 \
    libgstreamer1.0-0 \
    gstreamer1.0-plugins-good \
    gstreamer1.0-plugins-bad; \
    apt-get clean -y; \
    rm -rf /var/lib/apt/lists/* /var/cache/apt/*

COPY --from=build \
  /usr/local/src/neolink/target/release/neolink \
  /usr/local/bin/neolink

ENTRYPOINT [ "/usr/local/bin/neolink" ]
CMD [ "--config", "/etc/neolink.toml" ]

EXPOSE 8554