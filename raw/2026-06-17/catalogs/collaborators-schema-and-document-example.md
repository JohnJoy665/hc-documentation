---
title: "collaborators: поля каталога и пример документа сотрудника"
topic: "catalogs/collaborators"
source: "Сообщение пользователя от 2026-06-17"
status: "raw"
---

# collaborators: поля каталога и пример документа сотрудника

Источник: пользователь предоставил структуру таблицы `dbo.collaborators` и пример XML-документа существующего пользователя.

## Важное различие

- Каталог `collaborators` содержит поля, доступные для чтения.
- Для записи необходимо обращаться к документу сотрудника, открывая его.

## Поля каталога `collaborators`, доступные для чтения

Ключевые поля из предоставленного определения `dbo.collaborators`: `id`, `code`, `fullname`, `login`, `short_login`, `lowercase_login`, `email`, `phone`, `mobile_phone`, `birth_date`, `sex`, `pict_url`, `position_id`, `position_name`, `position_parent_id`, `position_parent_name`, `org_id`, `org_name`, `place_id`, `region_id`, `category_id`, `web_banned`, `is_arm_admin`, `is_content_admin`, `is_application_admin`, `role_id`, `is_candidate`, `candidate_status_type_id`, `candidate_id`, `is_outstaff`, `is_dismiss`, `position_date`, `hire_date`, `dismiss_date`, `in_request_black_list`, `allow_personal_chat_request`, `level_id`, `grade_id`, `knowledge_parts`, `tags`, `experts`, `person_object_profile_id`, `current_state`, `next_state_date`, `development_potential_id`, `efficiency_estimation_id`, `consent_kedo`, `consent_kedo_date`, `provider_legal_id`, `snils`, `cost_center_id`, `disp_birthdate`, `disp_birthdate_year`, `modification_date`, `app_instance_id`.

## Пример XML-документа сотрудника

В предоставленном примере документ имеет корневой элемент `collaborator` и содержит, среди прочего, поля:

- `id`
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
- `disp_*`
- `allow_personal_chat_request`
- `personal_chat_confirmation_required`
- `personal_config/default_info_type`
- `doc_info/creation/*`
- `doc_info/modification/*`
- `comp_ben/payment_period`
- `gdpr`
- `consent_kedo`
- `last_data/*`

## Нюансы

- `fullname` есть в каталоге, но в документе из примера ФИО хранится в полях `lastname`, `firstname`, `middlename`.
- Для создания нового документа в общих материалах подтверждена функция `OpenNewDoc(formUrl)`, но точный `formUrl` документа `collaborator` в локальной документации пока не подтвержден.
- Правила записи `password` требуют проверки на стенде: TODO: нужно проверить, допустима ли прямая запись или требуется стандартная функция платформы.

## TODO

- TODO: нужно проверить точный `formUrl` для создания нового документа `collaborator` через `OpenNewDoc(formUrl)`.
- TODO: нужно проверить, какие поля документа обязательны при создании сотрудника на конкретной версии WebSoft HCM.
- TODO: нужно проверить, нужно ли вручную заполнять `last_data` при создании или это делает стандартная логика формы/сохранения.
