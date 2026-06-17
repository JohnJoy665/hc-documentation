---
title: "Dev-pack и карта раздела WebSoft HCM: среда разработки"
topic: "development-map"
source_files:
  - "raw/2026-06-17/dev-pack/README.md"
  - "raw/2026-06-17/dev-pack/progress-report.md"
  - "raw/2026-06-17/dev-guide-map/websoft-hcm-dev-guide-map/00-guide-map.md"
status: "draft"
---

# Dev-pack и карта раздела WebSoft HCM: среда разработки

## Кратко

В `INBOX` были добавлены два архива:

- `websoft-hcm-dev-guide-map.tgz` - путеводитель по разделу «Websoft HCM: среда разработки»;
- `websoft-hcm-dev-pack.tgz` - AI-friendly пакет документации для разработки.

Оба архива сохранены и распакованы в `raw/2026-06-17/`.

## Темы из guide-map

Путеводитель содержит карту разделов:

- агент в системе WebSoft HCM;
- `global_settings` и `settings`;
- OmniCode;
- языки разработки;
- встроенные библиотеки;
- библиотеки программного кода;
- интерфейс и XAML;
- XQuery;
- внешние библиотеки;
- API v1 и API v3;
- редактор веб-страниц;
- функциональный дизайн выборок, удаленных действий и показателей;
- кэширование.

## Темы из dev-pack

Dev-pack содержит:

- `docs/00-overview.md` ... `docs/22-examples.md`;
- `api/api-v1-reference.md`;
- `api/api-v3-reference.md`;
- `libraries/*`;
- `xaml/xaml-elements.md`;
- `llms.txt`;
- `llms-full.md`;
- `sources.json`;
- `progress-report.md`;
- `raw/collected-pages.json`.

## Важное правило терминологии

В исходных материалах встречается термин `Server-Side JavaScript`. В рамках этого репозитория серверный код WebTutor не нужно автоматически отождествлять с ECMAScript-средами общего назначения, Node.js или браузерным кодом.

Для локальной документации предпочтительные формулировки:

- серверный код WebTutor;
- внутренний серверный язык WebTutor;
- WebTutor-подобный синтаксис;
- синтаксис, похожий на JavaScript, но выполняемый внутренним механизмом WebTutor.

Оригинальные названия источников можно сохранять как названия разделов, но при практической разработке нельзя переносить в WebTutor конструкции Node.js/npm/browser API без подтверждения локальной документацией.

## Частично покрытые области

По `progress-report.md` частично собраны:

- детальная документация по функциям и объектам внутреннего серверного языка;
- полный справочник `Tools`;
- библиотеки программного кода;
- API v3;
- интерфейс и XAML;
- редактор веб-страниц;
- функциональный дизайн;
- кэширование;
- API v1.

## TODO / вопросы для проверки

- TODO: нужно детально обработать функции `Tools`, `Tools_web`, `Tools_library`, `Ms_tools`.
- TODO: нужно собрать отдельные документы по `libMain`, `libAnalytics` и другим библиотекам.
- TODO: нужно расширить XAML-справочник.
- TODO: нужно проверить разделы API v1/v3 по полным первоисточникам.

## Источники

- `raw/2026-06-17/dev-pack/README.md`
- `raw/2026-06-17/dev-pack/progress-report.md`
- `raw/2026-06-17/dev-guide-map/websoft-hcm-dev-guide-map/00-guide-map.md`
