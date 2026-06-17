---
title: "SpXml.ini, xHttp.ini, SpXml2.ini, xHttp1.ini"
topic: "platform/configuration"
source_files:
  - "raw/2026-06-17/spxml-platform/configuration-files.txt"
status: "draft"
---

# SpXml.ini, xHttp.ini, SpXml2.ini, xHttp1.ini

При запуске платформа считывает конфигурационный файл рядом с исполняемым файлом:

- `SpXml.ini` - рабочее место;
- `xHttp.ini` - сервер.

Формат строки:

```text
<имя параметра>: <значение параметра>
```

Так как `SpXml.ini` и `xHttp.ini` входят в дистрибутив и могут затираться при обновлении, пользовательские параметры рекомендуется добавлять в `SpXml2.ini` и `xHttp1.ini`.

Основной параметр: `APPLICATION-MODULE`.
