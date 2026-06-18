---
title: "XQuery: for where return"
topic: "xquery/syntax"
source_files:
  - "raw/2026-06-17/xquery/spxml-xquery-query-syntax.md"
  - "raw/2026-06-17/xquery/xquery-syntax.txt"
status: "draft"
---

# XQuery: for where return

Базовая форма XQuery в SP-XML:

```xquery
for $elem in catalog
where <condition>
order by $elem/field descending
return $elem
```

Пример:

```xquery
for $elem in collaborators
where contains($elem/fullname, 'Иванов')
order by $elem/id ascending
return $elem
```

XQuery в SP-XML не следует считать полным стандартным XQuery без проверки.
