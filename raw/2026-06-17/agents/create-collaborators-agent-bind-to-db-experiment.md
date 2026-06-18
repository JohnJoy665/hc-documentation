---
title: "Создание сотрудников агентом: BindToDb и ошибки Empty document url"
topic: "agents/collaborators/create"
source: "Сообщение пользователя от 2026-06-17"
status: "raw"
---

# Создание сотрудников агентом: BindToDb и ошибки Empty document url

Источник: пользователь проверил агент создания сотрудников на сервере WebTutor / WebSoft HCM.

## Нерабочий вариант 1

Код:

```js
docUser = tools.new_doc_by_name("collaborator", false);
...
docUser.Save();
```

Ошибка:

```text
Ошибка при создании пользователя Мельникова Любовь Андреевна: Empty document url
(Save(),  x-local://wtv/wtv_server_agent.xmd,   line 316)
```

Такая же ошибка была для пользователей `Лебедев Савелий Петрович` и `Панина Ева Максимовна`.

Вывод из эксперимента: новый документ, созданный через `tools.new_doc_by_name("collaborator", false)`, перед сохранением нужно связать с базой данных.

## Нерабочий вариант 2

Код:

```js
docUser = tools.new_doc_by_name("collaborator", true);
```

Ошибка:

```text
Ошибка при создании пользователя Мельникова Любовь Андреевна: Unknown object property: lastname
(x-local://wtv/wtv_server_agent.xmd,   line 309)
```

Такая же ошибка была для пользователей `Лебедев Савелий Петрович` и `Панина Ева Максимовна`.

Вывод из эксперимента: для создания объекта сотрудника нужно использовать второй аргумент `false`, а не `true`. Значение `true` создает запись каталога/структуру, где нет свойства `lastname`.

## Рабочий вариант

Код:

```js
docUser = tools.new_doc_by_name( 'collaborator', false);
docUser.BindToDb( DefaultDb );
teUser = docUser.TopElem;
```

После `BindToDb(DefaultDb)` документ получает связь с базой данных, id/url и может быть сохранен через `docUser.Save()`.

## BindToDb

- Дата создания: 23.04.2021.
- Обновлено: 24.01.2023.
- Статус: активен.
- Назначение: преобразует документ, как правило вновь созданный, в объектный с присвоением нового `id` и соответствующего `url`.
- При создании нового документа нужно связать его с базой данных. Если документ не связан с базой данных, выводится ошибка `Empty document url`.

Синтаксис:

```text
BindToDb ([<arg1>])
```

Аргументы:

- `<arg1>` - необязательный аргумент.
- Тип: строка.
- Имя базы данных. Для текущей базы данных может использоваться имя `DefaultDb`.

Возвращаемое значение отсутствует.

Примеры:

```js
doc.BindToDb();
requestDoc.BindToDb( DefaultDb );
```

Проверенный в агенте вариант для сотрудника:

```js
docUser = tools.new_doc_by_name( 'collaborator', false);
docUser.BindToDb( DefaultDb );
```
