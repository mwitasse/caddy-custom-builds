# syntax=docker/dockerfile:1
FROM caddy:2.9.1-builder AS builder

RUN xcaddy build \
    --with github.com/caddy-dns/cloudflare \
    --with github.com/WeidiDeng/caddy-cloudflare-ip

FROM caddy:2.9.1

COPY --from=builder /usr/bin/caddy /usr/bin/caddy

# Verify the custom binary works and show loaded plugins
RUN caddy version > /tmp/caddy-version.txt && \
    caddy list-modules --versions > /tmp/caddy-modules.txt

LABEL caddy.build.info.available="true"
LABEL caddy.build.files="/tmp/caddy-version.txt,/tmp/caddy-modules.txt"