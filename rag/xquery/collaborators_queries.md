---
title: "XQuery: collaborators"
topic: "xquery/collaborators"
source_files:
  - "raw/2026-06-17/xquery/spxml-xquery-query-syntax.md"
  - "raw/2026-06-17/xquery/xquery-syntax.txt"
status: "draft"
---

# XQuery: collaborators

Примеры запросов к каталогу `collaborators`:

```xquery
for $elem in collaborators
where contains($elem/fullname, 'Test')
return $elem
```

```xquery
for $elem in collaborators
where $elem/login = 'TestTestTest'
return $elem
```

```xquery
for $elem in collaborators
where $elem/id = 6511610587817075498
return $elem
```

TODO: нужно проверить полный список полей `collaborators` по карточке каталога.
