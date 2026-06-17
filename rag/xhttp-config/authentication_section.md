---
title: "xhttp_config.json Authentication"
topic: "xhttp-config/authentication"
source_files:
  - "raw/2026-06-17/pdf/xhttp-config-json.txt"
status: "draft"
---

# xhttp_config.json Authentication

Секция `Authentication` задает правила аутентификации xHTTP.

Параметры из PDF:

- `Host` - адреса, для которых применяется правило; `*` означает все;
- `Scheme` - схема;
- `Exclusions` - исключения;
- `TicketStoreExpiration` - время жизни `x-auth-id`;
- `BearerTokenAllowed` - возможность использовать bearer-токен;
- `LogoutPath` - путь logout;
- `RedirectLoggedOffUri` - страница после выхода.

При нескольких секциях последняя должна иметь `"Host": "*"`.
