---
title: "tools.new_doc_by_name"
topic: "code-libraries/tools"
source: "Сообщение пользователя от 2026-06-17"
status: "raw"
---

# tools.new_doc_by_name

Источник: пользователь предоставил описание функции библиотеки `tools`.

## Библиотека

`tools` - библиотека WebTutor / WebSoft HCM. В ней есть удобная функция `new_doc_by_name`.

## Функция

`new_doc_by_name`

- Дата создания: 17.04.2019.
- Обновлено: 08.05.2021.
- Статус: активен.
- Назначение: создает новый объект в указанном каталоге.

## Входные параметры

- `sCatalogNameParam` (`string`) - название каталога. Обычно указывается без `s` на конце.
- `bIsCatalogParam` (`bool`) - флаг, указывающий, что создается новая запись в каталоге (`true`) или новый объект (`false`). Обычно передается `false`.

## Возвращаемый результат

`Doc` нового объекта.

## Примеры вызова

```js
docObject = tools.new_doc_by_name( 'collaborator', false );
docObject = tools.new_doc_by_name( Ps.catalog_name, false );
```
