# Карта базы знаний WebSoft HCM / WebTutor

## Покрытые темы

### Установка и локальный контур

- `docs/installation/installation-pack-overview.md` - обзор install-pack.
- `docs/local-dev-contour/postgresql-study-stand.md` - проверенный локальный PostgreSQL-стенд.
- `rag/local-dev-contour/postgresql_study_stand.md` - атомарная заметка по PostgreSQL-стенду.

### Авторизация и logout

- `docs/authentication/logout-basic-cookie.md` - кейс logout при Basic Auth и решение через `cookie / Form-Based`.
- `rag/authentication/basic_auth_logout.md` - Basic Auth после logout.
- `rag/authentication/tools_web_user_init.md` - `tools_web.user_init(Request, Request.Query)`.
- `rag/authentication/ouserinit_access_not_auth.md` - `oUserInit.access` не равен авторизованному пользователю.
- `rag/nodes/auth_type_cookie_form_based.md` - тип авторизации узла.

### Серверные HTML-хендлеры и API routing

- `docs/server-pages/html-handlers-and-api-routing.md` - серверные HTML-хендлеры, `router.html`, `Request`, `Response`.
- `rag/server-pages/router_html.md` - `router.html` как backend endpoint.
- `rag/request/request_auth_user_id.md` - `Request.AuthUserID`.
- `rag/request/request_query_body.md` - `Request.Query` и `Request.Body`.
- `rag/response/response_write.md` - `Response.Write`.

### URL routing и x-local

- `docs/url-routing/x-local-and-short-urls.md` - `x-local://` и короткие URL.
- `rag/url-routing/x-local.md` - атомарная заметка по `x-local://`.

### xHTTP config

- `docs/xhttp-config/xhttp-config-json.md` - `xhttp_config.json`.
- `rag/xhttp-config/authentication_section.md` - секция `Authentication`.
- `rag/xhttp-config/response_headers_rules.md` - `ResponseHeadersRules`.

### Карта разработки

- `docs/development-map/dev-pack-overview.md` - dev-pack и guide-map.
- `rag/development-map/dev_pack_limitations.md` - ограничения dev-pack.

## Слабые или пустые области

- Полный справочник внутреннего серверного языка WebTutor.
- Полный справочник объектов `Request`, `Response`, `Session`.
- Детальные функции `Tools`, `Tools_web`, `Tools_library`, `Ms_tools`.
- Полные карточки объектов и каталогов.
- XQuery пока покрыт только через карту раздела, без отдельной документации.
- XAML и интерфейс пока покрыты только через карту раздела.
- PDF по карточке узла и настройке аутентификации узла требуют OCR/ручной проверки.

## Связи

- Logout зависит от типа авторизации узла и секции `Authentication` в `xhttp_config.json`.
- API-хендлеры зависят от корректной проверки пользователя через `tools_web.user_init`, `Env.curUserID`, `Request.AuthUserID`.
- `router.html` использует `Request.Query`, `Request.Body`, `Response.Write` и серверные пути `x-local://`.
- Локальный PostgreSQL-стенд связан с installation pack и темой external database.
