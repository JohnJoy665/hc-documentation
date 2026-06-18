---
title: "Каталог collaborators и документ сотрудника"
topic: "catalogs/collaborators"
source_files:
  - "raw/2026-06-17/catalogs/collaborators-schema-and-document-example.md"
  - "raw/2026-06-17/code-libraries/tools-new-doc-by-name.md"
  - "raw/2026-06-17/agents/create-collaborators-agent-bind-to-db-experiment.md"
status: "draft"
---

# Каталог collaborators и документ сотрудника

## Кратко

`collaborators` - каталог сотрудников. По предоставленным данным поля каталога доступны для чтения. Для записи нужно открывать XML-документ сотрудника и менять его поля через `TopElem`, затем сохранять документ.

Серверный код для такой операции должен писаться как код внутреннего серверного языка WebTutor, а не как обычный JavaScript/Node.js.

## Каталог для чтения

Подтвержденные пользовательским источником читаемые поля каталога включают `id`, `fullname`, `login`, `email`, поля должности, подразделения, организации, флаги доступа и статуса.

Пример чтения каталога, проверенный пользователем в агенте:

```js
aCollaborators = ArraySelectAll(XQuery("for $elem in collaborators return $elem"));

for (oCollaborator in aCollaborators)
{
    alert(oCollaborator.login);
}
```

## Документ для записи

Для записи используется документ сотрудника. В предоставленном XML-примере корневой элемент называется `collaborator`.

Основные поля документа из примера:

- `lastname`
- `firstname`
- `middlename`
- `email`
- `login`
- `password`
- `position_id`
- `position_name`
- `position_parent_id`
- `position_parent_name`
- `org_id`
- `org_name`
- `change_password`
- `is_candidate`
- `is_outstaff`
- `is_dismiss`
- `in_request_black_list`
- `access/access_level`
- `access/access_role`
- `access/is_arm_admin`
- `access/web_banned`
- `access/is_content_admin`
- `access/is_application_admin`
- `disp_empty_fields`
- `disp_personal_info`
- `disp_login`
- `disp_sex`
- `disp_desc`
- `disp_files`
- `disp_birthdate`
- `disp_birthdate_year`
- `disp_resume`
- `allow_personal_chat_request`
- `personal_chat_confirmation_required`
- `personal_config/default_info_type`
- `doc_info/creation/user_login`
- `doc_info/creation/date`
- `doc_info/modification/user_login`
- `doc_info/modification/user_id`
- `doc_info/modification/date`
- `comp_ben/payment_period`
- `gdpr`
- `consent_kedo`
- `last_data/login`
- `last_data/password`
- `last_data/access_role`
- `last_data/is_arm_admin`
- `last_data/is_application_admin`
- `last_data/is_content_admin`
- `last_data/in_request_black_list`
- `last_data/position_id`

## Важные нюансы

- `fullname` есть в каталоге, но в предоставленном документе сотрудника ФИО хранится отдельными полями `lastname`, `firstname`, `middlename`.
- Для создания нового сотрудника можно использовать пользовательски предоставленную функцию `tools.new_doc_by_name('collaborator', false)`, которая возвращает `Doc` нового объекта.
- Перед `Save()` новый документ сотрудника нужно связать с базой данных через `docUser.BindToDb(DefaultDb)`. Иначе в проверенном агенте возникала ошибка `Empty document url`.
- Для создания объекта сотрудника второй аргумент `tools.new_doc_by_name` должен быть `false`. При `true` в проверенном агенте возникала ошибка `Unknown object property: lastname`.
- Не нужно обращаться напрямую к таблице `dbo.collaborators` для записи сотрудника. По пользовательскому источнику запись выполняется через документ.
- Не подтверждено, нужно ли вручную заполнять `last_data` при создании нового сотрудника или эти поля обновляет стандартная логика формы/сохранения.

## Проверенные факты

- Каталог `collaborators` можно читать через `XQuery("for $elem in collaborators return $elem")`.
- Для надежного перебора результата XQuery можно использовать `ArraySelectAll(...)`.
- Поля каталога `collaborators` из пользовательского источника доступны для чтения.
- Для записи сотрудника требуется открывать документ.

## TODO / вопросы для проверки

- TODO: нужно проверить обязательные поля документа сотрудника при создании в конкретной версии WebSoft HCM.
- TODO: нужно проверить, должен ли агент задавать `id`, `doc_info`, `last_data` вручную, или это делает стандартный механизм документа.
- TODO: нужно проверить правила хранения поля `password`.

## Источники

- `raw/2026-06-17/catalogs/collaborators-schema-and-document-example.md`
- `raw/2026-06-17/code-libraries/tools-new-doc-by-name.md`
- `raw/2026-06-17/agents/create-collaborators-agent-bind-to-db-experiment.md`
