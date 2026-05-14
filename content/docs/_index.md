---
title: ""
description: "Lintune installation and configuration documentation."
weight: 1
draft: false
---
>[!WARNING]
>Early alpha — full installer working end-to-end with Keycloak, Mailcow, and Nextcloud. Vaultwarden integration planned.


**Open-source MSP platform.** One identity, one password, one URL — for every service you run for your customers.

Lintune wires together Keycloak, Mailcow, Nextcloud, and Vaultwarden behind a single SSO layer and gives MSPs a control plane to provision and manage all of it from one place.

```sh
curl -fsSL https://get.lintune.xyz | bash
```

---

## Documentation

| Section | Audience |
|---------|----------|
| [Admin](admin/) | MSP operators — install, configure, and manage the platform |
| [Tenant Portal](dash/) | Customer admins — manage users, mailboxes, groups, and Nextcloud |

---

## How it works

```
curl get.lintune.xyz | bash
        │
        ▼
  lintune-admin  ──SSH──▶  target server
  (control plane)             │
        │                     ├── Keycloak   (SSO & identity)
        │                     ├── Mailcow    (email)
        │                     ├── Nextcloud  (file storage)
        │                     └── Vaultwarden (passwords — planned)
        │
  lintune-dash
  (tenant portal)
```

The installer provisions everything and wires each service to Keycloak SSO automatically. MSPs manage realms (customers) from `lintune-admin`; tenant admins log in at `lintune-dash` to manage their own organisation.

---

## Repositories

| Repo | Description |
|------|-------------|
| [lintune-admin](https://github.com/lintune/lintune-admin) | Laravel control plane for MSP operators. Handles realm provisioning, remote installs over SSH, and service integration. |
| [lintune-dash](https://github.com/lintune/lintune-dash) | Laravel tenant portal. Scoped to a single realm per session — users, mailboxes, groups, and Nextcloud. |
| [lintune-uptimekuma](https://github.com/lintune/lintune-uptimekuma) | Uptime Kuma with a Lintune REST API for programmatic monitor management. |
| [get.lintune.xyz](https://github.com/lintune/get.lintune.xyz) | Bootstrap install script. Installs Docker, generates secrets, and starts the stack. |

---

## Stack

- **Identity** — Keycloak 26 with Organizations + Home IdP Discovery
- **Email** — Mailcow Dockerized
- **Files** — Nextcloud AIO with `user_oidc` for Keycloak SSO
- **Passwords** — Vaultwarden *(planned)*
- **Monitoring** — Uptime Kuma (Lintune fork)
- **Apps** — Laravel 11 + PHP 8.4 + MariaDB 11
- **Proxy** — Caddy (automatic SSL) or bring your own
