# syntax=docker/dockerfile:1

# Build-Argumente für Go- und Caddy-Version (global verfügbar)
ARG GO_VERSION=1.24
ARG CADDY_VERSION=2.9.1

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
    --with github.com/caddy-dns/duckdns

# Prüfe ob die Binary erstellt wurde
RUN ls -la /go/bin/caddy && chmod +x /go/bin/caddy

# Finales schlankes Caddy-Image - verwende die Variable statt hardcoded
FROM caddy:${CADDY_VERSION}

# ARG nochmals deklarieren für diesen Stage
ARG CADDY_VERSION
ENV CADDY_VERSION=${CADDY_VERSION}

# Kopiere die gebaute Caddy-Binary
COPY --from=builder /go/bin/caddy /usr/bin/caddy

# Verify the custom binary works and show loaded plugins
RUN caddy version > /tmp/caddy-version.txt && \
    caddy list-modules --versions > /tmp/caddy-modules.txt

LABEL caddy.build.info.available="true"
LABEL caddy.build.files="/tmp/caddy-version.txt,/tmp/caddy-modules.txt"