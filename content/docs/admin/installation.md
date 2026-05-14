---
title: "Installation"
description: "Install the Lintune platform"
weight: 10
draft: false
toc: true
---
### 1. Run the installer script

On the server that will host Lintune admin and the tenant dashboard, run:

```bash
curl -fsSL https://get.lintune.xyz | bash
```

The script will:

- Install Docker if not already present
- Ask whether to use **Caddy** as an automatic SSL reverse proxy (recommended), or expose ports for your own proxy
- Prompt for your public domain names
- Generate secrets and write `admin.env`, `dash.env`, and `docker-compose.yml` to `/opt/lintune/`
- Pull the images and start the stack:
    1. Lintune Admin
    2. Lintune Dashboard
    3. Lintune Backup
    4. MariaDB Server
    5. Uptime Kuma
    6. Caddy (if selected)

> Secrets are saved to `/opt/lintune/.env` (root-readable only). Application env files are `/opt/lintune/admin.env` and `/opt/lintune/dash.env`.

### 2. Complete setup in the browser

Once the script finishes, open your admin URL in a browser. You will be taken to the **setup wizard** automatically.

---

## The setup wizard

The wizard runs at `/install` and handles everything that would otherwise require touching a command line.

### Guided install (SSH)

The wizard connects to your servers over SSH to install services via Docker. You will **need** root SSH access — the installer pipes scripts directly over SSH and cannot use `sudo` with a password prompt. SSH credentials are used only for installation and are not stored.

#### Single server

All services run on one machine. You can choose:

- **This server** — installs Keycloak on the same host running Lintune admin (using `host.docker.internal` to reach the host from inside the Docker container). SSH must be enabled on the host.
- **Different server** — enter a remote IP address.

Provide SSH credentials, the Keycloak port (default `8080`), and optionally a **public URL** if Keycloak will sit behind a reverse proxy. When a public URL is given, Keycloak starts in production mode with proxy headers enabled.

Mailcow and Nextcloud are optional. Toggle them on to install on the same server.

#### Multi-server

Each service runs on a separate machine. You provide SSH credentials per server:

| Service | Notes |
|---------|-------|
| Keycloak | Required |
| Mailcow | Optional — dedicated server |
| Nextcloud | Optional — dedicated server |

The wizard installs each service in sequence with live terminal output streamed to the browser. Each stage has a Retry button.

#### What gets installed

| Service | Location | Default port |
|---------|----------|-------------|
| Keycloak | Docker Compose at `/opt/keycloak/` | `8080` (configurable) |
| Mailcow | Cloned to `/opt/mailcow-dockerized/` | Standard Mailcow ports |
| Nextcloud AIO | Docker at `/opt/nextcloud-aio/` | AIO admin `9080`, web `11000` |

> Docker is installed automatically on any server that doesn't already have it.

#### After installation

Once all services are running the wizard:

1. Waits for Keycloak's `/health/ready` endpoint (up to 90 s)
2. Creates the `lintune-admin` OIDC client in the Keycloak master realm
3. Creates a dedicated `lintune-service` account with admin role
4. Creates a broker realm for tenant SSO
5. Writes all generated credentials to the settings table
6. Redirects you to the login page

---

## Post-login wizard

If you skipped Mailcow or Nextcloud during guided install, a second wizard at `/super/wizard` is available after first login. It connects those services without re-running the SSH installer — use it when adding services later or when they're hosted separately.

---

## Repair mode

If the `lintune-service` account is deleted or its password changes:

1. Set `SETUP_COMPLETE=` (empty) in `/opt/lintune/admin.env`
2. Restart the lintune-admin container
3. Visit `/install/manual` — enter your Keycloak admin credentials to recreate the service account without touching the broker realm or OIDC client

---

## Further reading

- [Reverse proxy / Caddy](caddy)
- [Architecture overview](architecture)
- [Troubleshooting](troubleshooting)
- [Upgrading](upgrade)
