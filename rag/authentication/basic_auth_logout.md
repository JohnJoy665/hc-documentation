---
title: "Basic Auth после logout"
topic: "authentication/logout"
source_files:
  - "raw/2026-06-17/auth-logout/2026-06-16-logout-basic-to-cookie-node-auth.md"
status: "verified"
---

# Basic Auth после logout

Если после `/logout` браузер продолжает отправлять `Authorization: Basic ...`, WebTutor может заново определить старого пользователя.

В проверенном кейсе встроенный xHTTP logout удалял cookie `x-auth-id`, `user_login`, `user_password`, но не мог очистить Basic credentials браузера. Корневая причина была в типе авторизации узла `localhost`: он работал как Basic-узел.

Решение проверенного кейса: в карточке узла изменить тип авторизации на `cookie / Form-Based`.
