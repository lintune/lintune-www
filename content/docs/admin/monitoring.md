---
title: "Monitoring"
description: "Service health monitoring with Uptime Kuma"
weight: 40
draft: false
toc: true
---
Lintune includes built-in service health monitoring via a custom [Uptime Kuma](https://github.com/louislam/uptime-kuma) image (`lintune-uptimekuma`).

---

## How it works

Uptime Kuma starts automatically as part of the bootstrap stack. During the setup wizard, Lintune initialises it with a dedicated user and API key, then creates HTTP monitors for each service as it is installed.

Monitor management is fully automatic — you do not configure Uptime Kuma directly.

---

## Where status is shown

**Service Status page** (`/super/services`) — a card per monitor with a coloured badge (Up / Down / Unknown / Maintenance) and the monitor URL. Refreshes every 30 seconds.

**Navbar dots** — coloured dots give an at-a-glance health check from any page in lintune-admin.

**Tenant dashboard** — lintune-dash shows dots for services relevant to that tenant's realm. The Nextcloud AIO admin interface is hidden from tenants (admin-only).

---

## Monitor visibility

Monitors whose names include `aio` are flagged `admin_only` and appear only in lintune-admin. Tenant admins in lintune-dash see only the services that are relevant to their realm.

---

## API

The `lintune-uptimekuma` image exposes a REST API used internally by Lintune. All endpoints require HTTP Basic Auth (`api:<key>`) except the one-time setup call.

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/api/lintune/setup` | One-shot setup — creates the first user and returns an API key. |
| `GET` | `/api/lintune/monitors` | List active monitors with current status. |
| `POST` | `/api/lintune/monitors` | Create an HTTP monitor. |
| `DELETE` | `/api/lintune/monitors/:id` | Delete a monitor. |

The API key is stored encrypted in the shared database. It never appears in an env file.
