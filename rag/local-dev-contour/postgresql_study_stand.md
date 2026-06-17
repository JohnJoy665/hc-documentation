---
title: "Учебный PostgreSQL-контур WebSoft HCM"
topic: "local-dev-contour/postgresql"
source_files:
  - "raw/2026-06-17/local-postgresql/local-study-contour-wt-postgresql.txt"
status: "verified"
---

# Учебный PostgreSQL-контур WebSoft HCM

Учебный стенд был создан в копии `C:\WebSoft\WebSoftServer_PG_LOCAL_STUDY`, основная папка `C:\WebSoft\WebSoftServer` не изменялась.

Параметры базы:

```text
Database: websoft_hcm_local_study
User: websoft_hcm_local_study_user
Password: websoft_hcm_local_study_pass
Host: localhost
Port: 5432
```

Корректный тип БД в x-shell: `postgresql`, не `pgsql`.

Миграция `db migrate from xml` перенесла 21627 документов, `Migration Errors:0`.
