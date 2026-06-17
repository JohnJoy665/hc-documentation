---
title: "Install-pack WebSoft HCM: обзор"
topic: "installation"
source_files:
  - "raw/2026-06-17/install-pack/websoft-hcm-install-pack/README.md"
  - "raw/2026-06-17/install-pack/websoft-hcm-install-pack/llms.txt"
  - "raw/2026-06-17/install-pack/websoft-hcm-install-pack/docs/00-overview.md"
status: "draft"
---

# Install-pack WebSoft HCM: обзор

## Кратко

В `INBOX` был добавлен архив `websoft-hcm-install-pack-v2.tgz` и рядом распакованная папка `websoft-hcm-install-pack-v2`. Пакет содержит AI-friendly материалы по установке и обслуживанию WebSoft HCM.

Raw-копии сохранены в:

```text
raw/2026-06-17/install-pack/
```

## Состав пакета

В распакованной папке есть:

- `README.md`
- `llms.txt`
- `llms-full.md`
- `raw/pages.json`
- `docs/00-overview.md`
- `docs/01-technical-requirements.md`
- `docs/02-installation-options.md`
- `docs/03-install-via-docker.md`
- `docs/04-install-on-linux.md`
- `docs/05-install-on-windows.md`
- `docs/06-external-database.md`
- `docs/07-https.md`
- `docs/08-run-as-service.md`
- `docs/09-mail-settings.md`
- `docs/10-backup-and-maintenance.md`
- `docs/11-update.md`
- `docs/12-security.md`
- `docs/13-troubleshooting.md`
- `docs/14-local-postgresql-study-stand.md`

## Темы

Пакет покрывает:

- варианты установки;
- технические требования;
- установку через Docker;
- установку на Linux и Windows;
- внешнюю базу данных;
- HTTPS;
- запуск как службу;
- почтовые настройки;
- резервное копирование и обслуживание;
- обновление;
- безопасность;
- troubleshooting;
- локальный PostgreSQL-стенд.

## TODO / вопросы для проверки

- TODO: нужно проверить, какие документы install-pack являются полностью исходными, а какие уже переработаны агентом.
- TODO: нужно отдельно извлечь атомарные RAG-заметки из каждого раздела install-pack при следующем проходе.
- TODO: файл `docs/14-local-postgresql-study-stand.md` внутри распакованного пакета пустой; сведения по PostgreSQL взяты из отдельного текстового отчета.

## Источники

- `raw/2026-06-17/install-pack/websoft-hcm-install-pack/README.md`
- `raw/2026-06-17/install-pack/websoft-hcm-install-pack/llms.txt`
- `raw/2026-06-17/install-pack/websoft-hcm-install-pack/docs/`
