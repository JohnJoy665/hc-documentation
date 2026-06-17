---
title: "tools_web.user_init(Request, Request.Query)"
topic: "authentication/tools_web"
source_files:
  - "raw/2026-06-17/api-routing/webtutor-api-html-handlers-request-user-init-short-urls.txt"
  - "raw/2026-06-17/auth-logout/2026-06-16-logout-basic-to-cookie-node-auth.md"
status: "draft"
---

# tools_web.user_init(Request, Request.Query)

`tools_web.user_init(Request, Request.Query)` используется в серверном коде WebTutor для анализа HTTP-запроса, параметров, cookies, session и заголовков авторизации.

После вызова в проверочных материалах читали:

```html
<%
oUserInit = tools_web.user_init(Request, Request.Query);
Env = Request.Session.GetOptProperty("Env", ({}));
curUserID = Env.GetOptProperty("curUserID", null);
%>
```

`oUserInit.access` не является достаточным признаком авторизованного пользователя. Для API нужно проверять `Env.curUserID` и/или `Request.AuthUserID`.
