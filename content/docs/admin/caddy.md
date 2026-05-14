---
title: "Reverse Proxy"
description: "Running Lintune behind Caddy or another reverse proxy"
weight: 50
draft: false
toc: true
---
## Caddy (recommended)

The bootstrap script can configure [Caddy](https://caddyserver.com/) as an automatic SSL reverse proxy. If you chose Caddy during install, no further configuration is needed — Caddy handles certificate provisioning and renewal automatically via Let's Encrypt.

---

## Cloudflare / custom proxy

If you're running Lintune behind Cloudflare or another proxy, Keycloak needs to be explicitly told it is behind a proxy and what its public hostname is — otherwise it generates incorrect redirect URLs.

Update `/opt/keycloak/keycloak.conf`:

```properties
proxy-headers=xforwarded
hostname=auth.yourdomain.com
http-enabled=true
```

| Setting | Description |
|---------|-------------|
| `proxy-headers` | `xforwarded` — tells Keycloak to trust `X-Forwarded-*` headers from the proxy |
| `hostname` | Your public Keycloak domain |
| `http-enabled` | `true` — allows Keycloak to accept plain HTTP from the local proxy while the proxy handles public HTTPS |

After changing `keycloak.conf`, restart Keycloak:

```bash
cd /opt/keycloak && docker compose restart
```

---

## Lintune admin / dash

Both Laravel apps trust proxy headers when running in Docker. `APP_URL` in each env file must be set to the public HTTPS URL, and the proxy must forward `X-Forwarded-For` and `X-Forwarded-Proto` headers.

`KEYCLOAK_BASE_URL` must also match the public HTTPS URL and must **not** have a trailing slash — a trailing slash produces double-slash paths that Keycloak rejects.

```
# Correct
KEYCLOAK_BASE_URL=https://auth.yourdomain.com

# Wrong — trailing slash breaks Keycloak API calls
KEYCLOAK_BASE_URL=https://auth.yourdomain.com/
```
