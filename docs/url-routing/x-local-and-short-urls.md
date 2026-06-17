---
title: "x-local://, короткие URL и пути серверных файлов"
topic: "url-routing/x-local"
source_files:
  - "raw/2026-06-17/api-routing/webtutor-api-html-handlers-request-user-init-short-urls.txt"
  - "raw/2026-06-17/api-routing/webtutor_api_routing_chat_report.md"
status: "draft"
---

# x-local://, короткие URL и пути серверных файлов

## Кратко

`x-local://` в исследованных материалах рассматривается как внутренняя схема WebTutor для обращения к файлам из серверного кода. Ее нельзя использовать в браузерном HTML как обычный `src` или `href`.

Короткие URL вида `/_wt/...` используются порталом, но детали маршрутизации пока описаны частично.

## x-local://

Неверный клиентский пример:

```html
<script src="x-local://wt/web/js/script.js"></script>
```

Причина: браузер не понимает `x-local://`.

Серверный пример из материалов:

```html
<%
Server.Execute("x-local://wt/web/timesheet/methods.html");
%>
```

В отчете по `router.html` также встречается построение пути до backend-библиотеки:

```text
x-local://wt/web/ambassador/back/<class>.js
```

## Важные нюансы

В одном из отчетов указано ограничение: по документации `x-local://` указывает на папку клиента `WebTutorAdmin`, а обращение к серверной папке может зависеть от настройки `SHARED-DATABASE` в `SpXml.ini`.

Этот факт требует дополнительной проверки на реальном стенде, потому что в практическом коде источников `x-local://wt/web/...` использовался для серверных include/библиотек.

## Проверенные факты

- `x-local://` нельзя считать клиентским URL для браузера.
- `x-local://` встречается в серверных вызовах `Server.Execute(...)`.
- В API routing отчете `x-local://` используется для построения пути к backend-библиотеке.

## TODO / вопросы для проверки

- TODO: нужно проверить точное разрешение `x-local://` для `WebTutorAdmin` и `WebSoftServer`.
- TODO: нужно проверить влияние `SHARED-DATABASE` в `SpXml.ini` на разрешение путей.
- TODO: нужно собрать отдельные примеры коротких URL `/_wt/...` и их связи с объектами портала.

## Источники

- `raw/2026-06-17/api-routing/webtutor-api-html-handlers-request-user-init-short-urls.txt`
- `raw/2026-06-17/api-routing/webtutor_api_routing_chat_report.md`
