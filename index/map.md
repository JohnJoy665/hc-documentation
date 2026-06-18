# Карта базы знаний WebSoft HCM / WebTutor

## Дополнение от 2026-06-18: объектная модель, XMD и каталоги

- `docs/catalogs/object-model-and-catalogs.md` - база данных WebTutor, объектная модель, XML-представление объектов, XMD-файлы, каталоги, `person_hierarchys`, справочник каталогов версии `1.26.05.21`.
- `docs/catalogs/link-catalogs.md` - каталоги-связки как `view` поверх нескольких стандартных каталогов, использование в `XQuery`, список примеров для версии `331 (2021.2.3.331)`.
- `rag/catalogs/object_model_xmd_catalogs.md` - атомарная заметка по связи объектной модели, XML, XMD и каталогов.
- `rag/catalogs/person_hierarchys.md` - атомарная заметка по каталогу `person_hierarchys`.
- `rag/catalogs/catalogs_reference_1_26_05_21.md` - атомарная заметка по справочнику каталогов версии `1.26.05.21`.
- `rag/catalogs/websoft_hcm_collaborator_catalog.md` - атомарная заметка по каталогу `websoft.hcm.collaborator`.
- `rag/catalogs/link_catalogs.md` - атомарная заметка по каталогам-связкам.
- `docs/catalogs/collaborators.md` - дополнен связью с `websoft.hcm.collaborator` и вопросом о соотношении имен `collaborators`, `dbo.collaborators`, `websoft.hcm.collaborator`.

## Дополнение от 2026-06-18: XML-объекты

- `docs/spxml-objects/xml-doc-objects.md` - связная справка по объектам `XmlDoc`, `XmlElem`, `XmlMultiElem` во внутреннем серверном языке WebTutor / SP-XML.
- `rag/spxml-objects/xmldoc.md` - атомарная заметка по объекту `XmlDoc`, его свойствам и методам сохранения.
- `rag/spxml-objects/xmlelem_delete.md` - атомарная заметка по удалению XML-элементов и дочерних элементов.
- `rag/spxml-objects/xmlmultielem.md` - атомарная заметка по множественным XML-элементам `XmlMultiElem`.
- `rag/spxml-objects/safe_opt_methods.md` - атомарная заметка по безопасным `Opt*`-методам.
- `rag/spxml-functions/bind_to_db.md` - дополнена предупреждением, что `XmlDoc.BindToDb()` в новом источнике помечен как deprecated, но в локальном эксперименте создания `collaborator` через агент работал с `DefaultDb`.

## Покрытые темы

### Платформа SP-XML и внутренний серверный язык

- `docs/platform/spxml-platform-overview.md` - назначение SP-XML, состав платформы, базы, `x-local://`, `x-db-obj://`, конфигурационные файлы.
- `docs/platform/spxml-application-structure.md` - структура приложения, кэш документов, модули, библиотеки V1/V2.
- `docs/server-language/spxml-script-differences.md` - отличия SP-XML Script от внешних ECMAScript-сред.
- `rag/platform/spxml_platform.md` - SP-XML как платформа.
- `rag/platform/spxml_databases.md` - базы `data`, `data_local`, `trash`, URL документов.
- `rag/platform/configuration_files.md` - `SpXml.ini`, `xHttp.ini`, `SpXml2.ini`, `xHttp1.ini`.
- `rag/server-language/spxml_script_differences.md` - атомарная заметка по отличиям языка.

### Встроенные функции SP-XML

- `docs/spxml-functions/functions-overview.md` - обзор групп встроенных функций SP-XML.
- `rag/spxml-functions/string_bytes_vs_chars.md` - строки: байты и символы.
- `rag/spxml-functions/array_direct_vs_select_all.md` - `ArrayDirect()` и `ArraySelectAll()`.
- `rag/spxml-functions/xml_document_open_fetch_cache.md` - `OpenDoc()`, `FetchDoc()` и кэш.
- `rag/spxml-functions/object_property_set_opt_lock.md` - динамическое чтение/запись свойств.
- `rag/spxml-functions/json_encode_parse.md` - `EncodeJson()` и `ParseJson()`.
- `rag/spxml-functions/html_encode_plain_text.md` - HTML-кодирование и plain text.
- `rag/spxml-functions/code_execution_eval_family.md` - функции выполнения кода.
- `rag/spxml-functions/date_parse_format.md` - даты: разбор и форматирование.
- `rag/spxml-functions/url_load_put_exists.md` - URL-функции чтения/записи/проверки.
- `rag/spxml-functions/filesystem_read_write_delete.md` - обзор файловых функций.

### Базовые объекты SP-XML

- `docs/spxml-basic-objects/basic-objects-overview.md` - `Array`, `Object`, `String`, `Binary`, `DataCache`, `Error`, `MethodReference` и другие базовые объекты.
- `rag/spxml-basic-objects/object_getoptproperty.md` - безопасное чтение свойств через `GetOptProperty()`.
- `rag/spxml-basic-objects/array_basic_methods.md` - базовые методы `Array`.
- `rag/spxml-basic-objects/string_length_bytes.md` - `String.length` как длина в байтах.
- `rag/spxml-basic-objects/datacache_globalcache_globalstorage.md` - `DataCache`, `GlobalCache`, `GlobalStorage`.
- `rag/spxml-basic-objects/methodreference_callback.md` - `MethodReference` как ссылка на метод.
- `rag/spxml-basic-objects/binary_deprecated.md` - `Binary` как устаревший объект.

### XQuery

- `docs/xquery/xquery-syntax.md` - синтаксис XQuery в SP-XML / WebTutor.
- `rag/xquery/basic_for_where_return.md` - базовая форма `for/where/order by/return`.
- `rag/xquery/collaborators_queries.md` - примеры запросов к `collaborators`.
- `rag/xquery/literals_true_false_null_date.md` - `true()`, `false()`, `null()`, `date()`.
- `rag/xquery/matchsome_doc_contains.md` - `MatchSome()` и `doc-contains()`.
- `rag/xquery/fields_return.md` - возврат ограниченного набора полей через `Fields(...)`.

### Каталоги и документы

- `docs/catalogs/collaborators.md` - каталог `collaborators`, читаемые поля и документ `collaborator` для записи.
- `rag/catalogs/collaborators_read_fields.md` - атомарная заметка по читаемым полям `collaborators`.
- `rag/catalogs/collaborator_document_write.md` - атомарная заметка: запись сотрудника через документ, а не через таблицу каталога.
- `rag/spxml-functions/log_event.md` - `EnableLog`, `EnableLogExt`, `LogEvent` для журналирования.
- `rag/spxml-functions/bind_to_db.md` - `BindToDb(DefaultDb)` для связывания нового документа с базой данных.
- `rag/agents/create_collaborators_agent.md` - проверенный фрагмент агента создания сотрудников.

### Агенты сервера

- `docs/agents/server-agents.md` - агенты сервера WebTutor / WebSoft HCM.
- `docs/agents/create-collaborators-agent.md` - проверенный рецепт агента создания сотрудников через `tools.new_doc_by_name` и `BindToDb`.
- `rag/agents/server_agents.md` - базовая заметка по агентам.
- `rag/agents/agent_alert_diagnostics.md` - `alert(...)` как диагностический вывод агента.

### Динамические методы и свойства объектов

- `docs/code-libraries/object-dynamic-methods.md` - `CallObjectMethod`, `CallObjectMethodWithLock`, `GetObjectProperty`, `GetClassObjectPropertyNames`.
- `rag/code-libraries/call_object_method.md`
- `rag/code-libraries/call_object_method_with_lock.md`
- `rag/code-libraries/get_object_property.md`
- `rag/code-libraries/get_class_object_property_names.md`
- `docs/code-libraries/tools-library.md` - библиотека `tools`, включая `tools.new_doc_by_name`.
- `rag/code-libraries/tools_new_doc_by_name.md` - атомарная заметка по созданию нового объекта через `tools.new_doc_by_name`.

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

- Полный справочник внутреннего серверного языка WebTutor частично покрыт через SP-XML функции, но требует сверки по версиям.
- Полный справочник объектов `Request`, `Response`, `Session`.
- Детальные функции `Tools`, `Tools_web`, `Tools_library`, `Ms_tools`.
- Полные карточки объектов и каталогов.
- Настройка агентов в администраторе по PDF требует OCR/ручной проверки.
- XQuery покрыт базово; нужны карточки каталогов и проверка полного списка полей.
- XAML и интерфейс пока покрыты только через карту раздела.
- PDF по карточке узла и настройке аутентификации узла требуют OCR/ручной проверки.

## Связи

- Logout зависит от типа авторизации узла и секции `Authentication` в `xhttp_config.json`.
- API-хендлеры зависят от корректной проверки пользователя через `tools_web.user_init`, `Env.curUserID`, `Request.AuthUserID`.
- `router.html` использует `Request.Query`, `Request.Body`, `Response.Write` и серверные пути `x-local://`.
- Локальный PostgreSQL-стенд связан с installation pack и темой external database.
- Агенты сервера связаны с внутренним серверным языком WebTutor, `XQuery`, `OpenDoc`, `TopElem`, `Save()`.
- `CallObjectMethod` и `GetObjectProperty` связаны с API routing, где методы и свойства могут вызываться динамически.
- `FetchDoc()` связан с кэшем документов, описанным в структуре приложения.
- Строковые функции связаны с правилом о бинарно-совместимых строках SP-XML Script.
- `Object.GetOptProperty()` связан с безопасным чтением параметров в серверных HTML-хендлерах и агентах.
- `XQuery` связан с каталогами `collaborators`, `events`, `courses`, `assessments`, `test_learnings`.
- `collaborators` связан с XQuery для чтения каталога и с XML-документом `collaborator` для записи.
- `LogEvent` связан с агентами сервера, где нужен отдельный журнал выполнения.
- `tools.new_doc_by_name('collaborator', false)` связан с созданием нового документа сотрудника.
- `BindToDb(DefaultDb)` связан с `tools.new_doc_by_name(...)`: без привязки к базе `Save()` может вернуть `Empty document url`.
