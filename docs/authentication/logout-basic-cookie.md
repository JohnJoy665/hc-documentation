---
title: "Logout, Basic Auth и cookie/Form-Based авторизация"
topic: "authentication/logout"
source_files:
  - "raw/2026-06-17/auth-logout/2026-06-16-logout-basic-to-cookie-node-auth.md"
  - "raw/2026-06-17/api-routing/webtutor-api-html-handlers-request-user-init-short-urls.txt"
  - "raw/2026-06-17/pdf/xhttp-config-json.txt"
status: "draft"
---

# Logout, Basic Auth и cookie/Form-Based авторизация

## Кратко

Проверенный кейс на WebSoft HCM 2023.2.967: после перехода на `/logout` пользователь снова входил на портал без ввода логина и пароля, потому что узел `localhost` был настроен на Basic-авторизацию.

Встроенный logout xHTTP удалял cookie `x-auth-id`, `user_login`, `user_password`, но браузер продолжал отправлять HTTP-заголовок `Authorization: Basic ...`. После этого `tools_web.user_init(Request, Request.Query)` снова определял старого пользователя.

Решение в проверенном кейсе: изменить тип авторизации узла на `cookie / Form-Based`.

## Подробное описание

Исходное поведение:

```text
/logout
-> /loggedoff.html
-> повторный переход на портал
-> вход под старым пользователем без ввода логина/пароля
```

Ответ `/logout` содержал признаки встроенного xHTTP logout:

```http
HTTP/1.1 302 Found
Location: /loggedoff.html
Set-Cookie: x-auth-id=rm; expires=Sat, 01 Jan 2000 00:00:00 GMT; path=/
Set-Cookie: user_login=rm; expires=Sat, 01 Jan 2000 00:00:00 GMT; path=/
Set-Cookie: user_password=rm; expires=Sat, 01 Jan 2000 00:00:00 GMT; path=/
```

При этом следующий запрос снова содержал:

```http
Authorization: Basic ...
```

Basic Auth не очищается через `Set-Cookie`, потому что это не cookie. Браузер может продолжать отправлять сохраненные credentials.

## Связь с xhttp_config.json

В `xhttp_config.json` проверенного стенда была секция:

```json
"Authentication":
[
  {
    "Host": "*",
    "Scheme": "None",
    "Exclusions": [
      "/default",
      "/lrs/*",
      "/webtutor/*",
      "/verify.html",
      "/token_redirect_for_omnicode.html",
      "/mobileservice/datafeed.xml*"
    ],
    "TicketStoreExpiration": "01:00:00",
    "BearerTokenAllowed": true,
    "LogoutPath": "/logout",
    "RedirectLoggedOffUri": "/loggedoff.html"
  }
]
```

Эта настройка задает встроенный путь выхода `/logout` и страницу после выхода `/loggedoff.html`.

## Диагностика

Проверки, которые помогли найти причину:

1. В Network проверить `Authorization`, `Cookie`, `Set-Cookie`, `Location`.
2. Проверить ответ `/logout`.
3. Добавить в предполагаемый кастомный logout-файл диагностический заголовок через `Response.AddHeader(...)` и убедиться, выполняется ли этот файл.
4. Проверить результат `tools_web.user_init(Request, Request.Query)` после logout.
5. Проверить карточку узла и поле типа авторизации.

Диагностический серверный код WebTutor:

```html
<%
oUserInit = tools_web.user_init(Request, Request.Query);
Env = Request.Session.GetOptProperty("Env", ({}));

Response.Write("<pre>");
Response.Write("oUserInit.access = " + oUserInit.GetOptProperty("access", "") + "\n");
Response.Write("oUserInit.error_code = " + oUserInit.GetOptProperty("error_code", "") + "\n");
Response.Write("oUserInit.auth_type = " + oUserInit.GetOptProperty("auth_type", "") + "\n");
Response.Write("oUserInit.auth_login = " + oUserInit.GetOptProperty("auth_login", "") + "\n");
Response.Write("Env.curUserID = " + Env.GetOptProperty("curUserID", "") + "\n");
Response.Write("Request.AuthUserID = " + Request.AuthUserID + "\n");
Response.Write("</pre>");
%>
```

Критический признак проблемы:

```text
auth_type = basic
auth_login = старый пользователь
Request.AuthUserID заполнен
```

## Важные нюансы

- `oUserInit.access = true` не всегда означает, что пользователь авторизован.
- Для кастомных API нужно проверять фактического пользователя: `Env.curUserID` и/или `Request.AuthUserID`.
- `Authorization: Basic ...` после logout означает, что браузер все еще отправляет Basic credentials.
- Удаление `/default` из `Authentication.Exclusions` было проверено в кейсе и не решило проблему.

## Проверенные факты

- Встроенный `/logout` xHTTP удалял cookie `x-auth-id`, `user_login`, `user_password`.
- Кастомный logout-файл не выполнялся при запросе `/logout`, если путь перехватывался встроенным xHTTP logout.
- После смены типа авторизации узла на `cookie / Form-Based` пользователь после logout попадал на форму входа.
- После исправления `auth_type = cookie`, а `Env.curUserID` и `Request.AuthUserID` были пустыми.

## TODO / вопросы для проверки

- TODO: нужно проверить, какие дополнительные переменные карточки узла обязательны для `cookie / Form-Based` в разных версиях WebSoft HCM.
- TODO: нужно проверить точное поведение `/loggedoff.html` для `portal_auth_type = ntlm`.

## Источники

- `raw/2026-06-17/auth-logout/2026-06-16-logout-basic-to-cookie-node-auth.md`
- `raw/2026-06-17/api-routing/webtutor-api-html-handlers-request-user-init-short-urls.txt`
- `raw/2026-06-17/pdf/xhttp-config-json.txt`
