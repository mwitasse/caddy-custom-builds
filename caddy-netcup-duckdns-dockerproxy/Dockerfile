# syntax=docker/dockerfile:1

# Build-Argumente für Go- und Caddy-Version (global verfügbar)
ARG GO_VERSION=1.24
ARG CADDY_VERSION=2.10.0

# Builder-Stage
FROM golang:${GO_VERSION} AS builder

# ARG nochmals deklarieren für diesen Stage
ARG CADDY_VERSION
ENV CADDY_VERSION=${CADDY_VERSION}
ENV PATH="/go/bin:${PATH}"

# Debug: Variable ausgeben
RUN echo "Building with Caddy version: ${CADDY_VERSION}"

# xcaddy installieren
RUN go install github.com/caddyserver/xcaddy/cmd/xcaddy@latest

# Caddy + Module bauen mit explizitem Output-Pfad
RUN xcaddy build v${CADDY_VERSION} --output /go/bin/caddy \
    --with github.com/caddy-dns/netcup \
    --with github.com/caddy-dns/duckdns \
    --with github.com/lucaslorentz/caddy-docker-proxy/v2

# Prüfe ob die Binary erstellt wurde
RUN ls -la /go/bin/caddy && chmod +x /go/bin/caddy

# Finales schlankes Caddy-Image - verwende die Variable statt hardcoded
FROM caddy:${CADDY_VERSION}

# ARG nochmals deklarieren für diesen Stage
ARG CADDY_VERSION
ENV CADDY_VERSION=${CADDY_VERSION}

# Kopiere die gebaute Caddy-Binary
COPY --from=builder /go/bin/caddy /usr/bin/caddy

CMD ["caddy", "docker-proxy"]