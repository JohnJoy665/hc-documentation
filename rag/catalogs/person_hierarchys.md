---
title: "person_hierarchys"
topic: "catalogs/person-hierarchys"
source_files:
  - "raw/2026-06-18/catalogs/webtutor_database_object_model_catalogs.md"
status: "draft"
---

# person_hierarchys

`person_hierarchys` - каталог для быстрого поиска сотрудников, имеющих отношение к указанному подразделению.

Поиск может быть:

- иерархическим: по указанному подразделению и дочерним подразделениям;
- неиерархическим: только по указанному подразделению.

Каталог заполняется автоматически и регулярно обновляется.

Поля из источника:

- `id`;
- `collaborator_id`;
- `position_id`;
- `parent_sub_id`;
- `subdivision_id`;
- `ready`.

`subdivision_id` описан как XML-структура с ID текущего подразделения и всех родительских подразделений вверх по иерархии; тип указан как `XmlElem`.

Пример:

```js
XQuery( "for $hier in person_hierarchys return $hier" );
```

TODO: нужно проверить полную документацию по `person_hierarchys`, так как ссылка из источника не раскрыта.
