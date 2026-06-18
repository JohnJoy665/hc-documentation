---
title: "LogEvent и журналы"
topic: "spxml-functions/logging"
source_files:
  - "raw/2026-06-17/spxml-functions/spxml_misc_functions.md"
status: "draft"
---

# LogEvent и журналы

`EnableLog(name, enable)` включает или выключает заданный журнал. Если второй аргумент не указан, по описанию используется `true`.

```js
EnableLog("newUsers", true);
```

`EnableLogExt(name, options)` включает журнал с дополнительными опциями. Подтвержденные опции: `life-time`, `base-dir`, `use-std-event-prefix`, `header-str`.

`LogEvent(type, text)` пишет строку в файл лога определенного типа.

```js
LogEvent("newUsers", "Пользователь Иванов Иван Иванович создан");
```
