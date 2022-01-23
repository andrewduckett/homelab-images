# build the geoip plugin
FROM golang:1.17 as geoip

WORKDIR /go/src/app

RUN git clone https://github.com/a-bali/telegraf-geoip.git . && \
    go build -o geoip cmd/main.go

# build the plex plugin
FROM golang:1.17 AS plex-webhooks

WORKDIR /go/src/app

RUN git clone https://github.com/russorat/telegraf-webhooks-plex.git . && \
    make

FROM telegraf

COPY --from=geoip --chown=telegraf:telegraf /go/src/app/geoip /usr/bin/geoip 
COPY --from=plex-webhooks --chown=telegraf:telegraf /go/src/app/bin/webhook-plex /usr/bin/webhook-plex

# Maybe add this later: https://github.com/weldpua2008/docker-net-snmp
RUN apt update && \
    # Add smartctl
    apt install -y --no-install-recommends smartmontools lm-sensors && \
    \
    # clean up
    rm -rf /var/lib/apt/lists/*

