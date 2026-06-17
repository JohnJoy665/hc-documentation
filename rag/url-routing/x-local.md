---
title: "x-local://"
topic: "url-routing/x-local"
source_files:
  - "raw/2026-06-17/api-routing/webtutor-api-html-handlers-request-user-init-short-urls.txt"
  - "raw/2026-06-17/api-routing/webtutor_api_routing_chat_report.md"
status: "draft"
---

# x-local://

`x-local://` используется в серверном коде WebTutor для обращения к файлам, например в `Server.Execute(...)`.

Нельзя использовать `x-local://` как обычный клиентский `src` или `href`: браузер не понимает эту схему.

TODO: нужно проверить точные правила разрешения `x-local://` для `WebTutorAdmin`, `WebSoftServer` и настройки `SHARED-DATABASE` в `SpXml.ini`.
