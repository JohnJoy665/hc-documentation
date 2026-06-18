---
title: "collaborators: читаемые поля каталога"
topic: "catalogs/collaborators"
source_files:
  - "raw/2026-06-17/catalogs/collaborators-schema-and-document-example.md"
status: "draft"
---

# collaborators: читаемые поля каталога

`collaborators` - каталог сотрудников. По пользовательскому источнику поля каталога доступны для чтения.

Ключевые поля: `id`, `fullname`, `login`, `email`, `position_id`, `position_name`, `position_parent_id`, `position_parent_name`, `org_id`, `org_name`, `web_banned`, `is_arm_admin`, `is_content_admin`, `is_application_admin`, `is_candidate`, `is_outstaff`, `is_dismiss`, `in_request_black_list`, `allow_personal_chat_request`, `modification_date`.

Пример чтения:

```js
aCollaborators = ArraySelectAll(XQuery("for $elem in collaborators return $elem"));
```

Для записи сотрудника нельзя опираться только на каталог: нужно открывать документ сотрудника.
