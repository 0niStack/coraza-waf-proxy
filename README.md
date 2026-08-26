# Self-Hosted WAF (Caddy + Coraza + OWASP CRS)

A self-hosted Web Application Firewall you can drop in front of any app.
Caddy handles the reverse proxy, Coraza inspects traffic, and the OWASP Core
Rule Set blocks common attacks (SQLi, XSS, path traversal, etc.) before they
reach your app.

This is WAF-only — no backend app included. You point it at whatever you
want to protect.

## Layout

```
caddy-waf/
├── docker-compose.yml
├── .env.example
└── caddy/
    ├── Dockerfile      # builds Caddy with the Coraza WAF plugin
    └── Caddyfile        # WAF rules + reverse proxy config
```

## Setup

```bash
cp .env.example .env
# edit .env: set UPSTREAM to the app you're protecting
docker compose up -d --build
```

`UPSTREAM` can be:

```bash
UPSTREAM=host.docker.internal:5000     # app running on the VM host (default)
UPSTREAM=my-app:8000                   # a container on this compose network
UPSTREAM=192.168.1.50:8080             # any other reachable host/IP
```

Change it anytime in `.env`, then `docker compose up -d` — no rebuild needed.

## Test it

```bash
curl http://localhost/                                                # normal request
curl -i "http://localhost/?id=%27%20OR%20%271%27=%271"                 # blocked (SQLi)
docker compose logs -f waf                                             # watch it in real time
```
