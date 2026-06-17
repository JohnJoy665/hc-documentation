---
title: "oUserInit.access не равен авторизованному пользователю"
topic: "authentication/tools_web"
source_files:
  - "raw/2026-06-17/auth-logout/2026-06-16-logout-basic-to-cookie-node-auth.md"
status: "verified"
---

# oUserInit.access не равен авторизованному пользователю

В проверенном кейсе после исправления logout был результат:

```text
oUserInit.access = true
auth_type = cookie
auth_login =
Env.curUserID =
Request.AuthUserID =
```

Вывод: `oUserInit.access = true` может означать доступ к странице/сценарию, но не обязательно наличие авторизованного пользователя.

Для API проверять `Env.curUserID` и/или `Request.AuthUserID`.
