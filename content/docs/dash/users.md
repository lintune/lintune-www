---
title: "Users"
description: "Managing users in the tenant portal"
weight: 10
draft: false
toc: true
---
The Users section shows all users in your organisation's Keycloak realm. Tenant admins can create, edit, enable/disable, and delete users, and control access to Mailcow and Nextcloud per user.

---

## Login & realm routing

The portal is scoped to your organisation. When you log in, the system matches your email domain to your Keycloak realm automatically — you are redirected to your realm's login without having to choose it manually.

Your session is bound to your realm. You cannot access data from other organisations.

Session timeout is 5 minutes of inactivity.

---

## Creating a user

New users are created in Keycloak with a username, email address, first name, and last name. After creation you can optionally:

- Enable a mailbox (requires Mailcow to be enabled for your realm)
- Enable Nextcloud access

User limits are set by your MSP at the realm level — the portal will prevent creating users beyond the configured limit.

---

## Enable / disable

Disabling a user suspends their Keycloak account. They cannot log in to any service while disabled. Enabling restores access immediately.

Disabling a user does not remove their mailbox or Nextcloud account — those remain and are restored when the user is re-enabled.

---

## Deleting a user

Deleting a user removes them from:

- Keycloak (the account is permanently deleted)
- Mailcow (mailbox deleted if one exists)
- Nextcloud (account deleted if one exists)

This action cannot be undone.

---

## Mailbox and Nextcloud toggles

Each user row shows a toggle for mailbox and Nextcloud access. These are independent — a user can have a mailbox without Nextcloud, or Nextcloud without a mailbox.

See [Mailboxes](mailboxes) and [Nextcloud](nextcloud) for details on what each toggle does.
