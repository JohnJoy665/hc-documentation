---
title: "XQuery: return Fields"
topic: "xquery/return"
source_files:
  - "raw/2026-06-17/xquery/spxml-xquery-query-syntax.md"
status: "draft"
---

# XQuery: return Fields

Чтобы вернуть ограниченный набор полей, в источнике используется конструкция:

```xquery
for $elem in events
return $elem/Fields('name','type_id')
```

TODO: нужно проверить поддержку `Fields(...)` для нужного каталога и версии WebSoft HCM.
