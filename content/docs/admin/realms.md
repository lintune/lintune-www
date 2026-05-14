---
title: "Realms & Multi-tenancy"
description: "How Lintune models customers as Keycloak realms"
weight: 30
draft: false
toc: true
---

## What is a realm?

In Lintune, a **realm** is a customer. Each customer gets a dedicated [Keycloak](https://www.keycloak.org/) realm — an isolated identity namespace with its own users, groups, and SSO configuration.

Every user in a realm shares the same Nextcloud instance and the same Mailcow instance. A realm is never split across instances.

---

## The broker realm

A special **broker realm** acts as a central federation hub. All tenant realms are registered as identity providers inside it.

This is what gives Lintune its "one URL" property — every user, regardless of which customer they belong to, logs in at the same address. The broker realm inspects the email domain, finds the matching tenant realm, and redirects the user there automatically. No manual IdP picker, no per-tenant login page.

```
https://auth.yourdomain.com
        │
        ▼
  broker realm (email domain lookup)
        │
        ▼
  tenant realm ──► user authenticates
        │
        ▼
  redirected back to lintune-dash (session scoped to this realm)
```

---

## Creating a realm

In lintune-admin, creating a realm:

1. Enter a realm name (becomes the Keycloak realm identifier) and the customer's email domain
2. Lintune creates the Keycloak realm and registers it in the broker realm automatically
3. Optionally enable Mailcow — provisions the email domain and sets mailbox limits
4. Optionally enable Nextcloud — provisions a user group and wires OIDC access control
5. Create the initial tenant admin user — they can log in immediately

All Keycloak configuration (OIDC client, IdP registration, SSO wiring) is handled automatically.

---

## Per-realm service limits

Each realm can have independent limits configured by the MSP:

| Limit | Controls |
|-------|---------|
| Max users | How many Keycloak users can be created in the realm |
| Max mailbox users | How many users can have a mailbox enabled |
| Max Nextcloud users | How many users can have Nextcloud access enabled |

These limits are enforced in lintune-dash when tenant admins provision users and services.

---

## Multi-instance support

One Lintune install manages one Keycloak but can have multiple Nextcloud and Mailcow instances. New realms are auto-assigned to the default instance; the MSP can override per realm.

Adding a new instance is done through **Settings → Add service** in lintune-admin — the same SSH installer runs on the new server and registers it as available.

| Resource | Cardinality |
|----------|-------------|
| Keycloak | One per Lintune install |
| Mailcow | One or more; one assigned per realm |
| Nextcloud | One or more; one assigned per realm |

---

## Data model

```
servers:          id, label, host, ssh_user, ssh_port
server_services:  id, server_id, service (keycloak|mailcow|nextcloud), service_url, is_default
realms:           id, keycloak_realm, nextcloud_service_id (FK), mailcow_service_id (FK)
```

`is_default` on `server_services` marks which instance new realms are assigned to. Only one default per service type is active at a time.
