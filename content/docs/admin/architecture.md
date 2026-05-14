---
title: "Architecture"
description: "Lintune architecture overview"
weight: 20
draft: false
toc: true
---
## Repositories

| Repo | Purpose |
|------|---------|
| `lintune-admin` | Laravel super-admin portal — realm provisioning, remote installs via SSH, Keycloak/Mailcow/Nextcloud integration |
| `lintune-dash` | Laravel tenant portal — user, mailbox, and Nextcloud management scoped to one realm per session |
| `lintune-backup` | Backup service — configured from the admin portal |
| `lintune-uptimekuma` | Uptime Kuma with a REST API layer for programmatic monitor management |
| `get.lintune.xyz` | Bootstrap script — installs Docker, generates secrets, writes compose file, starts the stack |

---

## Deployment

One command on a fresh Linux server:

```bash
curl -fsSL https://get.lintune.xyz | bash
```

The script installs Docker, generates secrets, and starts the full stack via `docker compose`. Lintune is **Docker-only** — there is no bare-metal install path. Mailcow and Nextcloud AIO are Docker-only upstream projects, so this constraint is inherent to the stack.

After the script completes, the setup wizard in the browser handles everything else.

---

## How services are installed

Lintune SSHes into target servers and runs install scripts over the connection. Root SSH access is required — the installer pipes bash scripts directly and cannot prompt for a `sudo` password.

**Fresh installs only.** Lintune owns the full install of each service. Importing existing Keycloak, Mailcow, or Nextcloud instances is not supported — controlling the install is what makes automatic SSO wiring reliable.

The install is staged — Keycloak first, then Mailcow and Nextcloud optionally:

| Stage | What happens |
|-------|-------------|
| Keycloak | Docker Compose at `/opt/keycloak/`. Lintune creates the OIDC client, broker realm, and service account automatically. |
| Mailcow | Cloned from GitHub to `/opt/mailcow-dockerized/`. API key generated, injected, and captured before containers start. |
| Nextcloud AIO | Docker container at `/opt/nextcloud-aio/`. Configuration written directly to the AIO volume; OIDC wired to Keycloak automatically. |

Live terminal output streams to the browser during each stage via SSE. Each stage has a Retry button if something goes wrong.

---

## Multi-tenancy

**Realm = tenant.** Each customer gets a dedicated Keycloak realm — an isolated identity namespace with its own users, groups, and SSO configuration.

One Keycloak instance serves all realms through a **broker realm** — a central federation hub. When a user logs in, their email domain is matched to a realm and they are automatically redirected to the correct Keycloak realm. No manual IdP picker; no per-tenant login page.

```
user@company.com logs in
        │
        ▼
  broker realm (email domain lookup)
        │
        ▼
  company.com realm ──► Keycloak authenticates user
        │
        ▼
  lintune-dash session scoped to company.com
```

---

## Multi-instance support

One Lintune install manages one Keycloak (the shared identity backbone) but can run multiple Nextcloud and Mailcow instances on separate servers.

New realms are auto-assigned to the default instance of each service. The MSP can override the assignment per realm. Adding a new instance uses the **"Add service"** flow in settings — the same SSH installer runs on the new server and registers it.

| Resource | Cardinality |
|----------|-------------|
| Keycloak | One per Lintune install |
| Mailcow | One or more; one assigned per realm |
| Nextcloud | One or more; one assigned per realm |

---

## Database

lintune-admin and lintune-dash share a single MariaDB database (`lintune`). Key constraints:

- **All migrations live in lintune-admin.** Never create or run migrations in lintune-dash — lintune-dash has no `database/migrations/` directory by design.
- **Both apps share the same `APP_KEY`.** Encrypted values written by lintune-admin (API keys, credentials) must be decryptable by lintune-dash. The key is generated once and copied to both env files by the bootstrap script.
- **lintune-dash never writes schema.** It reads from and writes to tables provisioned by lintune-admin, but never alters structure.

---

## Design principles

- **Docker-only.** No bare-metal install path.
- **Fresh installs only.** No importing existing services.
- **Root SSH required.** Installer runs scripts over SSH directly.
- **SSO wiring is automatic.** Every service is wired to Keycloak during install.
- **Web-driven after bootstrap.** The only terminal step is the initial `curl | bash`.
- **One URL for end users.** All services federated under the broker realm; users log in once.
