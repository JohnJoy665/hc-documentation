---
title: "Библиотека tools"
topic: "code-libraries/tools"
source_files:
  - "raw/2026-06-17/code-libraries/tools-new-doc-by-name.md"
  - "raw/2026-06-17/agents/create-collaborators-agent-bind-to-db-experiment.md"
status: "draft"
---

# Библиотека tools

## Кратко

`tools` - библиотека WebTutor / WebSoft HCM. По пользовательскому источнику в ней есть функция `new_doc_by_name`, которая создает новый объект в указанном каталоге и возвращает `Doc` нового объекта.

Серверный код с вызовом `tools.*` относится к внутреннему серверному языку WebTutor и не должен автоматически трактоваться как Node.js или браузерный JavaScript.

## tools.new_doc_by_name

`tools.new_doc_by_name(sCatalogNameParam, bIsCatalogParam)` создает новый объект в указанном каталоге.

Параметры:

- `sCatalogNameParam` (`string`) - название каталога. Обычно указывается без `s` на конце.
- `bIsCatalogParam` (`bool`) - `true`, если создается новая запись в каталоге; `false`, если создается новый объект. Обычно передается `false`.

Возвращает: `Doc` нового объекта.

Примеры:

```js
docObject = tools.new_doc_by_name( 'collaborator', false );
docObject = tools.new_doc_by_name( Ps.catalog_name, false );
```

## Связь с collaborator

Для создания сотрудника можно использовать:

```js
docUser = tools.new_doc_by_name( 'collaborator', false );
```

После этого можно заполнять `docUser.TopElem` и сохранять документ через `docUser.Save()`.

Проверенный пользователем нюанс: для вновь созданного документа сотрудника нужно вызвать `BindToDb(DefaultDb)` перед `Save()`, иначе при сохранении может возникнуть ошибка `Empty document url`.

```js
docUser = tools.new_doc_by_name( 'collaborator', false);
docUser.BindToDb( DefaultDb );
teUser = docUser.TopElem;
```

При замене второго аргумента на `true` в проверенном кейсе возникла ошибка `Unknown object property: lastname`, потому что созданный объект не имел поля `lastname`.

## TODO / вопросы для проверки

- TODO: нужно проверить обязательные поля конкретного объекта перед массовым созданием.
- TODO: нужно проверить правила записи пароля в документ `collaborator` на конкретном стенде.

## Источники

- `raw/2026-06-17/code-libraries/tools-new-doc-by-name.md`
- `raw/2026-06-17/agents/create-collaborators-agent-bind-to-db-experiment.md`
