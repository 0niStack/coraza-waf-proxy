# Stage 1: build a custom Caddy binary with the Coraza WAF module compiled in
FROM caddy:2-builder AS builder

RUN xcaddy build \
    --with github.com/corazawaf/coraza-caddy/v2

# Stage 2: ship it in the normal, small Caddy runtime image
FROM caddy:2

COPY --from=builder /usr/bin/caddy /usr/bin/caddy
COPY Caddyfile /etc/caddy/Caddyfile
