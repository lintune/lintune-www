---
title: "Upgrading"
description: "How to upgrade Lintune"
weight: 70
draft: false
toc: true
---
Lintune runs fully in Docker. Upgrading is a pull-and-restart — no manual dependency or migration steps required.

## Steps

```bash
cd /opt/lintune
docker compose pull
docker compose up -d
```

Database migrations run automatically when the containers start.

---

## What gets updated

`docker compose pull` fetches the latest tagged images for all services in your `docker-compose.yml`:

- `lintune-admin` — admin portal
- `lintune-dash` — tenant portal
- `lintune-uptimekuma` — monitoring
- `lintune-backup` — backup service

MariaDB and Caddy are pinned to major versions and do not auto-update.

---

## Before upgrading

- Check the [release notes](https://github.com/lintune) for breaking changes before pulling.
- Back up the MariaDB database and the `/opt/lintune/` directory.

```bash
docker exec lintune-db mysqldump -u root -p lintune > lintune-backup-$(date +%Y%m%d).sql
```
