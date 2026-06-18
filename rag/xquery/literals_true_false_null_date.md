---
title: "XQuery literals: true false null date"
topic: "xquery/literals"
source_files:
  - "raw/2026-06-17/xquery/spxml-xquery-query-syntax.md"
status: "draft"
---

# XQuery literals: true false null date

В XQuery SP-XML для специальных значений используются функции-литералы:

- `true()`
- `false()`
- `null()`
- `date(...)`

Примеры:

```xquery
for $x in groups where $x/is_dynamic = true() return $x
```

```xquery
for $er in event_results where $er/is_assist = false() return $er
```

```xquery
for $elem in events where $elem/start_date > date('2011-06-27') return $elem
```
