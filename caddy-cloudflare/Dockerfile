# syntax=docker/dockerfile:1

ARG CADDY_VERSION=2.11.4

# Builder runs natively on the runner and cross-compiles for each target platform
FROM --platform=$BUILDPLATFORM caddy:${CADDY_VERSION}-builder AS builder

ARG TARGETOS
ARG TARGETARCH
ARG TARGETVARIANT

ENV CGO_ENABLED=0 \
    GOPROXY=https://proxy.golang.org|direct

RUN GOOS=${TARGETOS} GOARCH=${TARGETARCH} GOARM=${TARGETVARIANT#v} \
    xcaddy build \
    --with github.com/caddy-dns/cloudflare \
    --with github.com/WeidiDeng/caddy-cloudflare-ip

FROM caddy:${CADDY_VERSION}

COPY --from=builder /usr/bin/caddy /usr/bin/caddy

# Verify the custom binary works and show loaded plugins
RUN caddy version > /tmp/caddy-version.txt && \
    caddy list-modules --versions > /tmp/caddy-modules.txt && \
    echo "Build completed at $(date)" > /tmp/caddy-build.txt