---
title: "Конфигурация xhttp_config.json"
topic: "xhttp-config"
source_files:
  - "raw/2026-06-17/pdf/xhttp-config-json.txt"
  - "raw/2026-06-17/auth-logout/2026-06-16-logout-basic-to-cookie-node-auth.md"
status: "draft"
---

# Конфигурация xhttp_config.json

## Кратко

PDF `Конфигурация xhttp_config.json.pdf` был успешно извлечен в текст. Материал описывает параметры `xhttp_config.json` для сервера WebSoft HCM, работающего в режиме службы. В случае работы через IIS часть параметров настраивается на стороне IIS.

## Authentication

Пример смешанной аутентификации:

```json
"Authentication":
[
  {
    "Host": "wsportalwin.websoft.ru",
    "Scheme": "Negotiate"
  },
  {
    "Host": "*",
    "Scheme": "None",
    "Exclusions": ["/default", "/lrs/*", "/webtutor/*", "/verify.html"],
    "TicketStoreExpiration": "01:00:00",
    "BearerTokenAllowed": true,
    "LogoutPath": "/logout",
    "RedirectLoggedOffUri": "/loggedoff.html"
  }
]
```

Для работы `x-auth-id` и bearer-токенов приводится схема:

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

Пояснения из PDF:

- `Host` - для каких адресов применяется правило, `*` означает все.
- `TicketStoreExpiration` - время жизни `x-auth-id`.
- `BearerTokenAllowed` - возможность использовать bearer-токен.
- При использовании нескольких секций в `Authentication` последняя должна иметь `"Host": "*"`.

## XAuthIdOptions

Для работы `x-auth-id` с несколькими хостами одного домена описана секция `XAuthIdOptions`:

- `UseMultiXAuthId`
- `MultiXAuthIdExpiration`
- `SignXAuthId`
- `HashKey`
- `CompressMultiXAuthIdString`
- `EncryptSignature`
- `CookieSecure`
- `CookieName`

## HTTPS и TLS

Для HTTPS через сертификат из хранилища используется `HttpsInlineCertStore`. Для разных SSL-сертификатов используется `MySniEndpoint`.

Для настройки протоколов используются параметры:

- `Protocols`
- `SSLProtocols`

Возможные значения `Protocols`:

- `Http1`
- `Http2`
- `Http3`
- `Http1AndHttp2`
- `Http1AndHttp2AndHttp3`

В PDF указано: для HTTP/2 требуется TLS 1.2 и выше, для HTTP/3 - TLS 1.3.

## AllowedHosts, WorkerThreads, Limits

`AllowedHosts` ограничивает доступность сервера определенными доменными именами.

`WorkerThreads` задает минимальное и максимальное количество worker/IO threads.

`Limits` задает ограничения:

- `MaxConcurrentConnections`
- `MaxConcurrentUpgradedConnections`
- `MaxRequestBodySize`
- `MaxRequestHeaderCount`

## ResponseHeadersRules

`ResponseHeadersRules` позволяет убирать или добавлять HTTP-заголовки в ответах сервера.

Пример удаления:

```json
"ResponseHeadersRules": [
  {
    "Host": "*",
    "Remove": ["Server"]
  }
]
```

Пример добавления:

```json
"ResponseHeadersRules":
[
  {
    "Host": "*",
    "Add": {"<Имя_заголовка>": "<Значение_заголовка>"}
  }
]
```

## Обязательные строки с версии 2023.2

В PDF указано, что с версии 2023.2 обязательны:

```json
"DefaultTicketCacheLifeTime": 600,
"AspNetMappings":
[
  {
    "Enabled": true,
    "Assembly": "Websoft.Mapping.HCM.dll",
    "Type": "Websoft.Mapping.HCM",
    "Properties":
    {
      "UserIdExpirationSec": 60
    }
  }
]
```

`DefaultTicketCacheLifeTime` - время жизни локального кэша bearer-токена. `AspNetMappings` должен быть указан для обратной совместимости с компонентом `Websoft.Mapping.HCM`.

## TODO / вопросы для проверки

- TODO: нужно проверить актуальность каждого параметра для конкретной версии WebSoft HCM.
- TODO: нужно проверить валидность JSON-примеров из PDF, потому что в извлеченном тексте встречаются переносы и фрагменты комментариев.
- TODO: нужно проверить, где именно размещается `XAuthIdOptions` относительно секции `Authentication`.

## Источники

- `raw/2026-06-17/pdf/xhttp-config-json.txt`
- `raw/2026-06-17/auth-logout/2026-06-16-logout-basic-to-cookie-node-auth.md`
