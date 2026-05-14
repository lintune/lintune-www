---
title: "Troubleshooting"
description: "Common issues and fixes"
weight: 80
draft: false
toc: true
---

## "Invalid Keycloak credentials" even though credentials are correct

**Symptom:** The setup wizard returns *Invalid Keycloak credentials* even though the same credentials work in the Keycloak admin UI.

**Cause:** Keycloak's default SSL requirement is `external requests`, which blocks token requests from non-HTTPS origins — including internal connections from Lintune.

**Fix:** In Keycloak, go to **Realm Settings** → **General** → set **Require SSL** to `none`.

> Do this only temporarily when accessing Keycloak over plain HTTP. The correct long-term approach is to put Keycloak behind an HTTPS reverse proxy.

---

## "Request path not normalized" from Keycloak

**Symptom:** Setup fails with `missingNormalization: Request path not normalized`.

**Cause:** `KEYCLOAK_BASE_URL` has a trailing slash, producing double-slash paths like `//realms/master/...` that Keycloak rejects.

**Fix:** Remove the trailing slash from `KEYCLOAK_BASE_URL` in `/opt/lintune/admin.env`:

```
KEYCLOAK_BASE_URL=https://auth.yourdomain.com
```

---

## Setup wizard appears again after completing setup

**Symptom:** After the wizard finishes, visiting the admin URL redirects back to `/install`.

**Cause:** `SETUP_COMPLETE` was not written to the env file, or the container is reading a stale value.

**Fix:** Check `/opt/lintune/admin.env` on the host and confirm `SETUP_COMPLETE=true` is present. If it is, restart the container:

```bash
docker compose -f /opt/lintune/docker-compose.yml restart lintune-admin
```

---

## Tenant login fails with "Your email domain matches an organization but you don't have an account yet"

**Symptom:** A user from a provisioned tenant realm gets this message instead of being redirected to their realm's login.

**Cause:** The Keycloak Organization or its linked IdP was not configured correctly during realm provisioning — usually the `redirectMode` is missing or the IdP is not linked to the Organization.

**Fix:** In lintune-admin, go to the realm and use the **Repair** action. This rebuilds the broker realm federation for that realm without touching the tenant realm itself.
