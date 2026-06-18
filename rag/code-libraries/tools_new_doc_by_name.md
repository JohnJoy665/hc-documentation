---
title: "tools.new_doc_by_name"
topic: "code-libraries/tools"
source_files:
  - "raw/2026-06-17/code-libraries/tools-new-doc-by-name.md"
  - "raw/2026-06-17/agents/create-collaborators-agent-bind-to-db-experiment.md"
status: "draft"
---

# tools.new_doc_by_name

`tools.new_doc_by_name(sCatalogNameParam, bIsCatalogParam)` создает новый объект в указанном каталоге и возвращает `Doc` нового объекта.

Параметры:

- `sCatalogNameParam` - название каталога, обычно без `s` на конце.
- `bIsCatalogParam` - `true` для новой записи в каталоге, `false` для нового объекта; обычно передается `false`.

Пример создания документа сотрудника:

```js
docObject = tools.new_doc_by_name( 'collaborator', false );
```

Проверенный пользователем рабочий вариант для создания сотрудника:

```js
docObject = tools.new_doc_by_name( 'collaborator', false );
docObject.BindToDb( DefaultDb );
```

Без `BindToDb(DefaultDb)` при `Save()` была ошибка `Empty document url`. С `true` вторым аргументом была ошибка `Unknown object property: lastname`.
