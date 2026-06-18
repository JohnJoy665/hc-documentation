---
title: "collaborator: запись через документ"
topic: "catalogs/collaborators"
source_files:
  - "raw/2026-06-17/catalogs/collaborators-schema-and-document-example.md"
  - "raw/2026-06-17/agents/create-collaborators-agent-bind-to-db-experiment.md"
status: "draft"
---

# collaborator: запись через документ

По пользовательскому источнику поля `collaborators` доступны для чтения, а для записи нужно открывать документ сотрудника.

В предоставленном XML-примере корневой элемент документа: `collaborator`. Основные поля документа: `lastname`, `firstname`, `middlename`, `email`, `login`, `password`, `position_id`, `position_name`, `position_parent_id`, `position_parent_name`, `org_id`, `org_name`, `access/*`, `last_data/*`.

`fullname` есть в каталоге, но в XML-примере документ хранит ФИО отдельными полями `lastname`, `firstname`, `middlename`.

Пользователь предоставил удобную функцию библиотеки `tools`:

```js
docObject = tools.new_doc_by_name( 'collaborator', false );
```

Она создает новый объект в указанном каталоге и возвращает `Doc` нового объекта.

Проверенный рабочий вариант:

```js
docObject = tools.new_doc_by_name( 'collaborator', false );
docObject.BindToDb( DefaultDb );
```

Без `BindToDb(DefaultDb)` сохранение дало `Empty document url`. При `tools.new_doc_by_name("collaborator", true)` обращение к `lastname` дало `Unknown object property: lastname`.
