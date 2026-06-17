# Открытые вопросы

## PDF и OCR

- TODO: нужно проверить PDF `raw/2026-06-17/pdf/node-card.pdf` вручную или через OCR. Автоматическое извлечение текста дало неинформативный результат.
- TODO: нужно проверить PDF `raw/2026-06-17/pdf/node-authentication-settings.pdf` вручную или через OCR. Автоматическое извлечение текста дало неинформативный результат.

## Авторизация и узлы

- TODO: нужно проверить, какие дополнительные переменные карточки узла обязательны для `cookie / Form-Based` в разных версиях WebSoft HCM.
- TODO: нужно проверить точное поведение `loggedoff.html` для `portal_auth_type = ntlm`.
- TODO: нужно проверить, какие поля карточки узла соответствуют `portal_auth_type`.

## Серверные HTML-хендлеры и API

- TODO: нужно проверить полный список свойств `Request` и методов `Response`.
- TODO: нужно проверить, есть ли методы `Response.SetHeader`, `Response.ContentType` или аналоги.
- TODO: нужно проверить production-safe способ установки `Content-Type: application/json`.
- TODO: нужно проверить безопасный синтаксис белых списков для `class` и `action` во внутреннем серверном языке WebTutor.
- TODO: нужно проверить актуальность `DropFormsCache` и замену для новых версий.

## x-local и маршрутизация

- TODO: нужно проверить точное разрешение `x-local://` для `WebTutorAdmin` и `WebSoftServer`.
- TODO: нужно проверить влияние `SHARED-DATABASE` в `SpXml.ini` на разрешение `x-local://`.
- TODO: нужно собрать отдельные примеры коротких URL `/_wt/...` и их связи с объектами портала.

## xhttp_config.json

- TODO: нужно проверить актуальность параметров `xhttp_config.json` для конкретной версии WebSoft HCM.
- TODO: нужно проверить валидность JSON-примеров из PDF после ручной сверки.
- TODO: нужно проверить, где именно размещается `XAuthIdOptions` относительно секции `Authentication`.

## Локальный PostgreSQL-контур

- TODO: нужно проверить влияние ошибок `ProcessDocInCatalog() failed` на поиск и каталоги.
- TODO: нужно проверить причину отсутствия `libAnalytics` в учебном контуре.
- TODO: нужно описать безопасный rollback учебного PostgreSQL-контура.

## Dev-pack

- TODO: нужно детально обработать функции `Tools`, `Tools_web`, `Tools_library`, `Ms_tools`.
- TODO: нужно собрать отдельные документы по `libMain`, `libAnalytics` и другим библиотекам программного кода.
- TODO: нужно расширить XAML-справочник.
- TODO: нужно проверить разделы API v1/v3 по полным первоисточникам.
