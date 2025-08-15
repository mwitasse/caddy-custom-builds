# syntax=docker/dockerfile:1

# Build-Argumente für Go- und Caddy-Version
ARG GO_VERSION=1.24
ARG CADDY_VERSION=2.10.0

# Builder-Stage
FROM golang:${GO_VERSION} AS builder
ENV PATH="/go/bin:${PATH}"

# xcaddy installieren
RUN go install github.com/caddyserver/xcaddy/cmd/xcaddy@latest

# Caddy + Module bauen
RUN xcaddy build v${CADDY_VERSION} \
    --with github.com/caddy-dns/netcup \
    --replace github.com/libdns/netcup=github.com/Monviech/libdns-netcup@libdns-patch \
    --with github.com/caddy-dns/duckdns \
    --with github.com/lucaslorentz/caddy-docker-proxy/v2

# Finales schlankes Caddy-Image
FROM caddy:${CADDY_VERSION}
COPY --from=builder /caddy /usr/bin/caddy

CMD ["caddy", "docker-proxy"]
