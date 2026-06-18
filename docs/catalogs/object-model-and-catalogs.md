---
title: "База данных, объектная модель, XMD и каталоги"
topic: "catalogs/object-model"
source_files:
  - "raw/2026-06-18/catalogs/webtutor_database_object_model_catalogs.md"
status: "draft"
---

# База данных, объектная модель, XMD и каталоги

## Кратко

WebTutor / WebSoft HCM хранит данные на логическом уровне как объекты разных типов. У объекта есть XML-представление, а структура объектов и каталогов описывается XMD-документами.

Каталог - это таблица/индекс, по которому можно искать объекты через `XQuery`. Для каждого типа объекта существует как минимум один каталог. Структура каталога может отличаться от структуры полного XML-документа объекта.

## Поддерживаемые типы баз данных

По источнику WebTutor поддерживает:

- Microsoft SQL Server, начиная с 2008, оптимально 2012;
- Oracle 11+;
- PostgreSQL;
- XML как внутреннее хранилище;
- Microsoft Azure при облачном размещении.

## Объекты и XML

Любой объект представляет иерархическую структуру данных и имеет XML-представление. Атрибуты объекта могут быть:

- скалярными: текст, число, дата, булево значение;
- массивами: содержащими скалярные значения или другие массивы;
- ссылками на другие объекты в базе.

В источнике приведен пример XML-структуры объекта `lector` с полями `id`, `type`, `desc`, `person_id`, `person_fullname`, `doc_info`, `access` и другими.

## XMD-файлы

XMD-файлы описывают структуру типов объектов и хранятся в папке сервера:

```text
\wtv\wtv_<тип объекта>.xmd
```

В XMD могут задаваться поля, типы, обязательность, значения по умолчанию, ссылки на другие каталоги, наследование базовых структур и вычисляемые свойства.

Примеры конструкций из источника:

```xml
<person_id TYPE="integer" FOREIGN-ARRAY="collaborators" TITLE="const=c_coll"/>
<allow_publication TYPE="bool" NOT-NULL="1" DEFAULT="false" TITLE="const=vleb_can_public"/>
<INHERIT TYPE="doc_info_base"/>
```

В XMD также могут быть блоки с выражениями, например `OnBeforeSave`, `DocDesc`, вычисляемые свойства. Такой код нужно рассматривать как серверный код WebTutor во внутреннем серверном языке платформы, а не как обычный JavaScript.

## Каталоги

Для поиска объектов используются каталоги. Источник содержит справочник каталогов версии `1.26.05.21 (2026-05-21)`.

Примеры каталогов из источника:

- `collaborator` - сотрудник;
- `server_agent` - агент сервера;
- `custom_web_template` - настраиваемый Web-шаблон;
- `websoft.hcm.collaborator` - сотрудник;
- `websoft.hcm.person_object_link` - контент сотрудника;
- `websoft.platform.devtools.devtool_catalog` - "Каталоги".

Полный список каталогов сохранен в raw-источнике.

## Каталог `websoft.hcm.collaborator`

Источник содержит таблицу данных каталога `websoft.hcm.collaborator` (`Сотрудник`). Среди полей:

- `id`;
- `code`;
- `fullname`;
- `login`;
- `short_login`;
- `lowercase_login`;
- `email`;
- `phone`;
- `mobile_phone`;
- `birth_date`;
- `sex`;
- `position_id`;
- `position_name`;
- `position_parent_id`;
- `position_parent_name`;
- `org_id`;
- `org_name`;
- `place_id`;
- `region_id`;
- `web_banned`;
- `is_arm_admin`;
- `is_content_admin`;
- `is_application_admin`;
- `role_id`;
- `is_candidate`;
- `is_outstaff`;
- `is_dismiss`;
- `hire_date`;
- `dismiss_date`;
- `level_id`;
- `grade_id`;
- `knowledge_parts`;
- `tags`;
- `experts`;
- `current_state`;
- `current_state_id`;
- `development_potential_id`;
- `efficiency_estimation_id`;
- `consent_kedo`;
- `snils`;
- `cost_center_id`;
- `modification_date`;
- `app_instance_id`.

Часть полей содержит ссылки на другие каталоги, например `position_id` на `websoft.hcm.positions`, `position_parent_id` на `websoft.hcm.subdivisions`, `org_id` на `websoft.hcm.orgs`.

TODO: нужно проверить соответствие имен `websoft.hcm.positions`/`websoft.hcm.position`, `websoft.hcm.subdivisions`/`websoft.hcm.subdivision`, `websoft.hcm.orgs`/`websoft.hcm.org` в конкретной версии и контексте запроса.

## Каталог `person_hierarchys`

`person_hierarchys` предназначен для быстрого поиска сотрудников, имеющих отношение к указанному подразделению. Поиск может быть иерархическим, включая дочерние подразделения, или неиерархическим, только по указанному подразделению.

Каталог заполняется автоматически и регулярно обновляется.

Поля из источника:

- `id`;
- `collaborator_id`;
- `position_id`;
- `parent_sub_id`;
- `subdivision_id`;
- `ready`.

Поле `subdivision_id` описано как XML-структура с ID текущего подразделения и всех родительских подразделений вверх по иерархии. Тип поля в источнике указан как `XmlElem`.

## Примеры XQuery из источника

```js
XQuery( "for $hier in person_hierarchys return $hier" );
```

```js
XQuery( "for $hier in person_hierarchys where MatchSome($hier/subdivision_id,(" + ArrayMerge(aCatalogFMs, "This.parent_id.Value", ",") + ")) and $hier/collaborator_id != " + iPersonID + " return $hier/Fields('collaborator_id')" );
```

Эти примеры относятся к серверному коду WebTutor. Блоки кода в источнике были размечены как `javascript`, но это не делает их обычным JavaScript.

## Важные нюансы

- Структура каталога может отличаться от структуры XML-документа объекта.
- Физическая структура базы может различаться для разных СУБД.
- При первом запуске сервера после установки дистрибутива физическая структура данных актуализируется на основе XMD-файлов.
- Иллюстрации, упомянутые в исходном материале, в `INBOX` не переданы.
- Ссылка "здесь" в описании `person_hierarchys` в присланном материале не раскрыта.

## Проверенные факты

- WebTutor использует XMD для описания структуры типов объектов и каталогов.
- Поиск объектов выполняется через каталоги с помощью `XQuery`.
- `person_hierarchys` автоматически заполняется и регулярно обновляется.
- Источник содержит справочник каталогов версии `1.26.05.21 (2026-05-21)`.

## TODO / вопросы для проверки

- TODO: нужно проверить точные имена каталогов со множественным/единственным числом в `FOREIGN`-ссылках.
- TODO: нужно проверить полную документацию по `person_hierarchys`, так как ссылка "здесь" в источнике не раскрыта.
- TODO: нужно проверить, какие поля каталога `websoft.hcm.collaborator` доступны в конкретной версии и чем они отличаются от ранее полученной схемы `dbo.collaborators`.

## Источники

- `raw/2026-06-18/catalogs/webtutor_database_object_model_catalogs.md`
