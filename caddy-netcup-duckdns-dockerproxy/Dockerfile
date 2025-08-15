# syntax=docker/dockerfile:1

# Stage 1: determine suitable Go-version from Caddy's go.mod
FROM alpine:3 AS goversion
RUN apk add --no-cache curl grep
ARG CADDY_VERSION=2.10.0
RUN curl -sSL "https://raw.githubusercontent.com/caddyserver/caddy/v${CADDY_VERSION}/go.mod" \
    | grep -E '^go [0-9]+\.[0-9]+' \
    | cut -d ' ' -f 2 \
    > /go-version.txt

# Stage 2: Use builder with determined Go-version
FROM golang:$(cat /go-version.txt) AS builder
ARG CADDY_VERSION=2.10.0

# install xcaddy
RUN go install github.com/caddyserver/xcaddy/cmd/xcaddy@latest
ENV PATH="/go/bin:${PATH}"

# Build caddy + modules
RUN xcaddy build v${CADDY_VERSION} \
    --with github.com/caddy-dns/netcup \
    --replace github.com/libdns/netcup=github.com/Monviech/libdns-netcup@libdns-patch \
    --with github.com/caddy-dns/duckdns \
    --with github.com/lucaslorentz/caddy-docker-proxy/v2

# Stage 3: Final slim caddy image
FROM caddy:${CADDY_VERSION}

COPY --from=builder /caddy /usr/bin/caddy

CMD ["caddy", "docker-proxy"]
