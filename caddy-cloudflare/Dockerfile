# syntax=docker/dockerfile:1
FROM caddy:2.10.2-builder AS builder

RUN xcaddy build \
    --with github.com/caddy-dns/cloudflare \
    --with github.com/WeidiDeng/caddy-cloudflare-ip

FROM caddy:2.10.2

COPY --from=builder /usr/bin/caddy /usr/bin/caddy

# Verify the custom binary works and show loaded plugins
RUN caddy version > /tmp/caddy-version.txt && \
    caddy list-modules --versions > /tmp/caddy-modules.txt && \
    echo "Build completed at $(date)" > /tmp/caddy-build.txt
