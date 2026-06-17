---
title: "Серверные HTML-хендлеры и backend API routing"
topic: "server-pages/api-routing"
source_files:
  - "raw/2026-06-17/api-routing/webtutor-api-html-handlers-request-user-init-short-urls.txt"
  - "raw/2026-06-17/api-routing/webtutor_api_routing_chat_report.md"
status: "draft"
---

# Серверные HTML-хендлеры и backend API routing

## Кратко

В WebTutor / WebSoft HCM серверный код может выполняться внутри `.html`-файлов, размещенных в `wt/web`. Такие файлы могут открываться напрямую по URL и использоваться как серверные HTML-хендлеры или backend API endpoints.

Код в таких файлах написан на внутреннем серверном языке WebTutor с WebTutor-подобным синтаксисом. Его нельзя автоматически отождествлять с ECMAScript-средами общего назначения, браузерным кодом или Node.js.

## Где выполняется серверный код

Исследованные материалы фиксируют несколько вариантов:

- прямой HTML-хендлер в `wt/web`;
- HTML-файл как код шаблона портала;
- единый `router.html`, который принимает HTTP-запрос и вызывает backend-библиотеки;
- серверные include-файлы, подключаемые через `Server.Execute(...)`.

## Паттерн router.html

Практическая модель из отчета:

```text
router.html
  -> подключает user_init.html
  -> читает Request.Query и Request.Body
  -> получает class/action/data
  -> открывает x-local://.../<class>.js через OpenCodeLib
  -> вызывает action через CallObjectMethod
  -> возвращает JSON через Response.Write
```

Пример из источника описывает параметры:

- `class` - имя backend-библиотеки;
- `action` - имя метода;
- `data` - данные запроса;
- `curUser` - текущий пользователь или auth context, который нужно передавать явно.

## Request и Response

В материалах выделены объекты и методы:

- `Request`
- `Request.Query`
- `Request.Body`
- `Request.AuthUserID`
- `Request.Session`
- `Request.DropSession`
- `Response`
- `Response.Write`

`Request.Query` используется для чтения параметров URL и POST-формы. `Request.Body` используется для чтения тела HTTP-запроса строкой.

`Response.Write(...)` записывает строку в HTTP-ответ. Для возврата JSON в источнике использовался подход:

```html
<%
Response.Write(tools.object_to_text(actionResult, "json"));
%>
```

## Авторизация в API-хендлерах

Для определения пользователя в исследованных материалах использовались:

```html
<%
oUserInit = tools_web.user_init(Request, Request.Query);
Env = Request.Session.GetOptProperty("Env", ({}));
curUserID = Env.GetOptProperty("curUserID", null);
%>
```

Важно: `oUserInit.access` не является достаточным признаком авторизованного пользователя. Для API нужно проверять `Env.curUserID` и/или `Request.AuthUserID`.

## Риски паттерна

Из источника по `router.html`:

- динамический `class` опасен без белого списка;
- динамический `action` опасен без проверки разрешенных методов;
- поведение `x-local://` зависит от контекста и требует проверки;
- `DropFormsCache` нельзя считать актуальным решением для новых версий без проверки;
- в отчете отмечено, что не хватает подтверждения по установке `Content-Type`.

## Проверенные факты

- `.html`-файл в `wt/web` может использоваться как серверный endpoint.
- `Server.Execute(AppDirectoryPath() + "/wt/web/include/user_init.html")` используется для подключения серверного include-файла `user_init.html`.
- `tools_web.user_init(Request, Request.Query)` анализирует текущий HTTP-запрос, параметры, cookies, session и заголовки авторизации.
- `x-local://` нельзя использовать в клиентском HTML как обычный `src` или `href`.

## TODO / вопросы для проверки

- TODO: нужно проверить точный полный список свойств `Request` и методов `Response` по официальным карточкам объектов.
- TODO: нужно проверить, есть ли методы `Response.SetHeader`, `Response.ContentType` или их аналоги.
- TODO: нужно проверить production-safe способ установки `Content-Type: application/json`.
- TODO: нужно проверить безопасный синтаксис белых списков для `class` и `action` во внутреннем серверном языке WebTutor.

## Источники

- `raw/2026-06-17/api-routing/webtutor-api-html-handlers-request-user-init-short-urls.txt`
- `raw/2026-06-17/api-routing/webtutor_api_routing_chat_report.md`
