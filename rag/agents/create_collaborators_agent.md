---
title: "Агент создания сотрудников"
topic: "agents/collaborators/create"
source_files:
  - "raw/2026-06-17/agents/create-collaborators-agent-bind-to-db-experiment.md"
status: "draft"
---

# Агент создания сотрудников

Проверенный пользователем рабочий фрагмент для создания сотрудника в агенте:

```js
docUser = tools.new_doc_by_name( 'collaborator', false);
docUser.BindToDb( DefaultDb );
teUser = docUser.TopElem;

teUser.lastname = nameParts[0];
teUser.firstname = nameParts[1];
teUser.middlename = nameParts[2];
teUser.login = "user" + curN;
teUser.password = "user" + curN;

docUser.Save();
```

Нюансы:

- `tools.new_doc_by_name("collaborator", false)` без `BindToDb(DefaultDb)` дал ошибку `Empty document url` при `Save()`.
- `tools.new_doc_by_name("collaborator", true)` дал ошибку `Unknown object property: lastname`.
- Для объекта сотрудника в этом кейсе нужен второй аргумент `false`.
