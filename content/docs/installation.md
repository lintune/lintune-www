---
title: "Installation"
description: "Installation guide on how to install the lintune suite"
weight: 100
draft: false
toc: true
---

###
### 1. Run the installer script

On the server that will host Lintune admin and the tenant dashboard, run:

```bash
curl -fsSL https://get.lintune.xyz | bash
```

The script will:

- Install Docker if not already present
- Ask whether to use **Caddy** as an automatic SSL reverse proxy (RECOMMENDED), or expose ports for your own proxy
- Prompt for your public domain names (or URLs)
- Generate secrets and write `admin.env`, `dash.env`, and `docker-compose.yml` to `/opt/lintune/`
- Pull the images and start the stack:
    1. Lintune Admin
    2. Lintune Dashboard
    3. Lintune Backup
    4. MariaDB Server
    5. caddy (if selected)
    6. Uptime Kuma

> Secrets are saved to `/opt/lintune/.env` (root-readable only). Application env files are `/opt/lintune/admin.env` and `/opt/lintune/dash.env`.

### 2. Complete setup in the browser

Once the script finishes, open your admin URL in a browser. You will be taken to the **setup wizard** automatically.

---

## The setup wizard

The wizard runs in the browser at `/install` and handles everything that would otherwise require touching a command line.

---

### Guided install (SSH)

The wizard connects to your servers over SSH to install services via Docker. You wil **NEED** root account and password. These details are **NOT** stored anywhere and are only used for the installation.

#### Single server

All services run on one machine. You can choose:

- **This server** — installs Keycloak on the same host running Lintune admin (using `host.docker.internal` to reach the host from inside the Docker container). SSH must be accessible on the host.
- **Different server** — enter a remote IP address.

Provide SSH credentials (username + password), the Keycloak port (default `8080`), and optionally a **public URL** if Keycloak will sit behind a reverse proxy (e.g. `https://keycloak.company.com`). When a public URL is given, Keycloak is started in production mode with reverse-proxy headers enabled.

Mailcow and Nextcloud are optional. Toggle them on to install on the same server.

#### Multi server

Each service runs on a separate machine. You provide SSH credentials per server:

| Service | Default SSH host field | Notes |
|---|---|---|
| Keycloak | `kc_host` | Required |
| Mailcow | `mc_host` | Optional — needs its own box |
| Nextcloud | `nc_host` | Optional — needs its own box |

The wizard installs each service in sequence, reporting live output as it runs.

#### What gets installed

| Service | How | Default port |
|---|---|---|
| Keycloak | Docker Compose at `/opt/keycloak/` | `8080` (configurable) |
| Mailcow | Cloned from GitHub to `/opt/mailcow-dockerized/` | Standard Mailcow ports |
| Nextcloud AIO | Docker container | AIO admin on `9080`, web on `11000` |

> Docker is installed automatically on any server that doesn't already have it.
> Mailcow requires `git`, `curl`, `jq`, and `openssl` — the wizard installs any that are missing.

#### After installation

Once all services are running the wizard:

1. Waits for Keycloak's `/health/ready` endpoint (up to 90 s)
2. Creates the `lintune-admin` OIDC client in the Keycloak master realm
3. Creates a dedicated `lintune-service` account with admin role
4. Creates a broker realm for tenant SSO
5. Writes all generated credentials to `.env` and the settings table
6. Redirects you to the login page

---

## Repair mode

If the `lintune-service` account is deleted or its password changes:

1. Set `SETUP_COMPLETE=` (empty) in `.env`
2. Run `php artisan config:clear`
3. Visit `/install/manual` — enter your Keycloak admin credentials to recreate the service account without touching the broker realm or OIDC client

---

## Further reading

- [Reverse proxy setup](proxy.md)
- [Architecture overview](architecture.md)
- [Troubleshooting](troubleshooting.md)
- [Upgrading](upgrade.md)