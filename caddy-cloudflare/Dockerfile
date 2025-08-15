# syntax=docker/dockerfile:1

FROM caddy:2.10.0-builder AS builder

RUN xcaddy build \
    --with github.com/caddy-dns/cloudflare \
    --with github.com/WeidiDeng/caddy-cloudflare-ip


FROM caddy:2.10.0

# Copy custom Caddy binary over the official one
COPY --from=builder /usr/bin/caddy /usr/bin/caddy

# Verify the custom binary works and show loaded plugins
RUN caddy version > /tmp/caddy-version.txt && \
    caddy list-modules --versions > /tmp/caddy-modules.txt

LABEL caddy.build.info.available="true"
LABEL caddy.build.files="/tmp/caddy-version.txt,/tmp/caddy-modules.txt"

# The rest is inherited from the official Caddy image:
# - USER caddy  
# - EXPOSE 80 443 2019
# - VOLUME ["/config", "/data"]
# - WORKDIR /srv
# - CMD ["caddy", "run", "--config", "/etc/caddy/Caddyfile", "--adapter", "caddyfile"]