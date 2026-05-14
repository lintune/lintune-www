---
title: "Groups"
description: "Managing groups in the tenant portal"
weight: 30
draft: false
toc: true
---
Groups let you organise users for email distribution and access control. There are two group types.

---

## Mailing lists

A mailing list is backed by a [Mailcow](https://mailcow.email/) shared alias. Email sent to the list address is delivered to all members' mailboxes.

- Members must have an active mailbox
- The alias is created when the first member is added and deleted when all members are removed
- Requires Mailcow to be enabled for your realm

---

## Security groups

A security group is synced to Keycloak. If Nextcloud is enabled for your realm, security groups are also synced to Nextcloud as groups — useful for controlling access to Team Folders.

---

## Managing members

Members are added via a picker that shows all users in your realm. Each entry is identified by their email address. Adding or removing a member updates the underlying Mailcow alias or Keycloak group immediately.
