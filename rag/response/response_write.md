---
title: "Response.Write"
topic: "response-object"
source_files:
  - "raw/2026-06-17/api-routing/webtutor_api_routing_chat_report.md"
status: "draft"
---

# Response.Write

`Response.Write(...)` записывает строку в HTTP-ответ серверного HTML-хендлера.

Для JSON в источнике использовался пример:

```html
<%
Response.Write(tools.object_to_text(actionResult, "json"));
%>
```

TODO: нужно проверить корректный способ установки `Content-Type: application/json`.
