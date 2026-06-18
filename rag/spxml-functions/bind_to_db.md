---
title: "BindToDb"
topic: "spxml-functions/xml-document"
source_files:
  - "raw/2026-06-17/agents/create-collaborators-agent-bind-to-db-experiment.md"
status: "draft"
---

# BindToDb

`BindToDb([arg1])` преобразует документ, обычно вновь созданный, в объектный документ с присвоением нового `id` и `url`.

Если новый документ не связан с базой данных, при `Save()` возможна ошибка:

```text
Empty document url
```

`arg1` необязателен. Это имя базы данных; для текущей базы можно использовать `DefaultDb`.

Пример:

```js
docUser = tools.new_doc_by_name( 'collaborator', false);
docUser.BindToDb( DefaultDb );
```
