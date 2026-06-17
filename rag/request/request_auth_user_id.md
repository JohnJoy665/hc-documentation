---
title: "Request.AuthUserID"
topic: "request-object"
source_files:
  - "raw/2026-06-17/api-routing/webtutor-api-html-handlers-request-user-init-short-urls.txt"
  - "raw/2026-06-17/auth-logout/2026-06-16-logout-basic-to-cookie-node-auth.md"
status: "draft"
---

# Request.AuthUserID

`Request.AuthUserID` использовался в диагностике авторизации как признак фактически определенного пользователя.

После проблемного logout при Basic Auth поле было заполнено старым пользователем. После смены узла на `cookie / Form-Based` после logout поле стало пустым.

Для API можно проверять `Request.AuthUserID` вместе с `Env.curUserID`.
