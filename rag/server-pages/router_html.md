---
title: "router.html как backend endpoint"
topic: "server-pages/api-routing"
source_files:
  - "raw/2026-06-17/api-routing/webtutor_api_routing_chat_report.md"
status: "draft"
---

# router.html как backend endpoint

`router.html` может использоваться как единая точка входа backend API:

```text
router.html -> user_init -> Request.Query/Body -> class/action/data -> backend library -> Response.Write
```

Риски:

- динамический `class` нужен ограничивать белым списком;
- динамический `action` нужен ограничивать списком разрешенных методов;
- `curUser` лучше передавать в методы явно;
- поведение `x-local://` требует проверки на стенде.
