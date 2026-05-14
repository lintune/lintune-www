---
title: "Mailboxes"
description: "Managing email mailboxes in the tenant portal"
weight: 20
draft: false
toc: true
---
Mailboxes are backed by [Mailcow](https://mailcow.email/). Each user can have one mailbox tied to their email address.

Mailbox management is only available if your MSP has enabled Mailcow for your realm.

---

## Enabling a mailbox

Enabling a mailbox for a user creates the mailbox in Mailcow using the user's email address as the mailbox address. The user can then configure their email client using standard IMAP/SMTP settings.

Mailbox limits (total allowed per realm) are set by your MSP. The portal will prevent enabling mailboxes beyond the configured limit.

---

## Disabling a mailbox

Disabling a mailbox deactivates it in Mailcow. Existing emails are retained — they are not deleted. The user cannot send or receive email while the mailbox is disabled.

Re-enabling the mailbox restores full access to existing email.

---

## What happens on user deletion

When a user is deleted, their mailbox is permanently deleted from Mailcow including all stored email. This cannot be undone.
