---
title: "BindToDb"
topic: "spxml-functions/xml-document"
source_files:
  - "raw/2026-06-17/agents/create-collaborators-agent-bind-to-db-experiment.md"
  - "raw/2026-06-18/xml-objects/xml-doc-objects-xmldoc-ai-friendly.md"
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

## Статус метода

В источнике по `XmlDoc` от 2026-06-18 метод `BindToDb()` помечен как deprecated.

Для новой объектной модели источник предлагает проверять `DefaultDb.OpenNewObjectDoc()` или `XmlDoc.BindToDbObjectType()`.

При этом в локально проверенном эксперименте создания `collaborator` через агент рабочим оказался путь:

```js
docUser = tools.new_doc_by_name( 'collaborator', false);
docUser.BindToDb( DefaultDb );
docUser.Save();
```

TODO: нужно проверить, какой способ создания нового объектного документа рекомендуется для конкретной версии WebSoft HCM / WebTutor.
