---
title: "websoft.hcm.collaborator"
topic: "catalogs/collaborator"
source_files:
  - "raw/2026-06-18/catalogs/webtutor_database_object_model_catalogs.md"
status: "draft"
---

# websoft.hcm.collaborator

`websoft.hcm.collaborator` - каталог `Сотрудник` из справочника каталогов источника.

Источник фиксирует поля каталога, включая:

- `id`;
- `code`;
- `fullname`;
- `login`;
- `short_login`;
- `lowercase_login`;
- `email`;
- `phone`;
- `mobile_phone`;
- `birth_date`;
- `sex`;
- `position_id`;
- `position_name`;
- `position_parent_id`;
- `position_parent_name`;
- `org_id`;
- `org_name`;
- `place_id`;
- `region_id`;
- `web_banned`;
- `is_arm_admin`;
- `is_content_admin`;
- `is_application_admin`;
- `role_id`;
- `is_candidate`;
- `is_outstaff`;
- `is_dismiss`;
- `hire_date`;
- `dismiss_date`;
- `level_id`;
- `grade_id`;
- `knowledge_parts`;
- `tags`;
- `experts`;
- `current_state`;
- `current_state_id`;
- `development_potential_id`;
- `efficiency_estimation_id`;
- `consent_kedo`;
- `snils`;
- `cost_center_id`;
- `modification_date`;
- `app_instance_id`.

Некоторые поля являются ссылками на другие каталоги. Например, `position_id`, `position_parent_id`, `org_id`, `place_id`, `region_id`, `role_id`, `level_id`, `grade_id`, `current_state_id`.

TODO: нужно проверить соответствие этого каталога ранее зафиксированному `dbo.collaborators` и рабочему имени `collaborators` в `XQuery`.
