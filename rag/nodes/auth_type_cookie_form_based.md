---
title: "Тип авторизации узла cookie / Form-Based"
topic: "nodes/authentication"
source_files:
  - "raw/2026-06-17/auth-logout/2026-06-16-logout-basic-to-cookie-node-auth.md"
status: "verified"
---

# Тип авторизации узла cookie / Form-Based

В проверенном кейсе для локального стенда смена типа авторизации узла `localhost` на `cookie / Form-Based` решила проблему повторного входа после logout без ввода логина и пароля.

После исправления форма входа показывала поля логина и пароля, а `Request.AuthUserID` после logout был пустым.

TODO: нужно проверить обязательные дополнительные переменные карточки узла для разных версий WebSoft HCM.
