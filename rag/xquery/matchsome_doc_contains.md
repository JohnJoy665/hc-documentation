---
title: "XQuery: MatchSome и doc-contains"
topic: "xquery/functions"
source_files:
  - "raw/2026-06-17/xquery/spxml-xquery-query-syntax.md"
status: "draft"
---

# XQuery: MatchSome и doc-contains

`MatchSome()` проверяет попадание значения в набор:

```xquery
for $elem in events
where MatchSome($elem/type_id, ('one_time', 'real_time'))
return $elem
```

`doc-contains()` используется для полнотекстового поиска и поиска по кастомным полям:

```xquery
for $elem in candidates
where doc-contains($elem/id, 'data', 'xxx')
return $elem
```

Условия по кастомным полям с операторами сравнения зависят от СУБД: источник указывает MS SQL/PostgreSQL как поддерживаемые, XML-база не поддерживает такой вариант.
