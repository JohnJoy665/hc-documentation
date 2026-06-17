---
title: "Агенты сервера WebTutor"
topic: "agents/server-agents"
source_files:
  - "raw/2026-06-17/agents/wt_server_agents_server_side_js.md"
status: "draft"
---

# Агенты сервера WebTutor

Агенты сервера запускают серверные процедуры по расписанию.

Административный путь из источника:

```text
Дизайнер -> Бизнес-логика -> Агенты сервера
```

Код агента пишется без `<% ... %>`. Файл может иметь расширение `.js`, но это серверный код WebTutor / SP-XML Script, а не код для Node.js или браузера.

В примере агента использовались `XQuery`, `OpenDoc`, `UrlFromDocID`, `TopElem`, `tools.create_notification`, `Save()` и `alert(...)`.
