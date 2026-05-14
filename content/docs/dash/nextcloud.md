---
title: "Nextcloud"
description: "Managing Nextcloud access in the tenant portal"
weight: 40
draft: false
toc: true
---
Nextcloud access is opt-in per user. It is only available if your MSP has enabled Nextcloud for your realm.

---

## Enabling access

Enabling Nextcloud for a user does two things:

1. Adds the user to the `nextcloud` group in Keycloak
2. Pre-provisions a Nextcloud account using the user's email address as their Nextcloud username

The user can log in to Nextcloud immediately using the same SSO credentials they use for everything else — no separate Nextcloud password.

Nextcloud user limits are set by your MSP. The portal will prevent enabling access beyond the configured limit.

---

## Disabling access

Disabling Nextcloud for a user:

1. Removes them from the `nextcloud` Keycloak group
2. Deletes their Nextcloud account and all stored files

> **This permanently deletes all of the user's Nextcloud files.** Ensure files are backed up or transferred before disabling access.

---

## How access control works

Nextcloud uses Keycloak as its identity provider via OIDC. At login, Nextcloud checks the user's token for membership in the `nextcloud` group. Users who are not in the group are blocked at the Nextcloud layer before an account is created — even if they try to access Nextcloud directly.

This means access control is enforced by Keycloak group membership, not by Nextcloud's own user database. Enabling/disabling access in lintune-dash takes effect immediately on the next login.
