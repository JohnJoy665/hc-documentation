---
title: "XQuery в SP-XML / WebTutor"
topic: "xquery/syntax"
source_files:
  - "raw/2026-06-17/xquery/spxml-xquery-query-syntax.md"
  - "raw/2026-06-17/xquery/xquery-syntax.txt"
status: "draft"
---

# XQuery в SP-XML / WebTutor

## Кратко

XQuery в SP-XML используется для выборки данных из каталогов базы данных WebTutor / WebSoft HCM. В источнике указано, что используется не полный стандартный XQuery, а ограниченное подмножество синтаксиса, дополненное платформенными функциями.

Основная форма:

```xquery
for $elem in catalog
where <condition>
order by $elem/field descending
return $elem
```

## Общая форма запроса

```xquery
for <декларация переменных>
(where <условие>)
(order by <имя поля> (descending | ascending))
return <возвращаемое значение>
```

Круглые скобки в описании означают необязательные части.

Пример:

```xquery
for $elem in events
where $elem/status_id = 'plan'
order by $elem/start_date descending
return $elem
```

## Переменные и каталоги

Переменная начинается с `$`.

```xquery
for $x in groups return $x
```

Можно объявлять несколько переменных для связи каталогов:

```xquery
for $e in education_methods, $x in events
where $x/education_method_id = $e/id and $x/status_id = 'plan'
return $e
```

## Условия where

Поддерживаются логические операторы:

- `and`
- `or`

Поддерживаются операторы сравнения:

- `=`
- `!=`
- `>`
- `>=`
- `<`
- `<=`

Пример по двум критериям из PDF:

```xquery
for $elem in collaborators
where (contains($elem/fullname, 'Test') and $elem/login = 'TestTestTest')
return $elem
```

## Сортировка

```xquery
for $elem in collaborators
where contains($elem/fullname, 'Иванов')
order by $elem/id ascending
return $elem
```

```xquery
for $elem in collaborators
where contains($elem/fullname, 'Иванов')
order by $elem/id descending
return $elem
```

Если направление сортировки важно, лучше указывать `ascending` или `descending` явно.

## return

Можно вернуть всю запись:

```xquery
for $elem in events return $elem
```

Можно вернуть поле:

```xquery
for $elem in events return $elem/name
```

Можно вернуть ограниченный набор полей:

```xquery
for $elem in events return $elem/Fields('name','type_id')
```

## Функции внутри XQuery

### contains()

Ищет подстроку в строке. По источнику игнорирует регистр.

```xquery
for $emp in collaborators
where contains($emp/position_name, 'инженер')
return $emp
```

Предупреждение из источника: `contains()` может быть медленным, потому что не оптимизируется индексами встроенной СУБД.

### doc-contains()

Используется для полнотекстового поиска и поиска по кастомным полям.

```xquery
for $elem in candidates
where doc-contains($elem/id, 'data', 'xxx')
return $elem
```

Для кастомных полей:

```xquery
doc-contains($elem/id, DefaultDb, '[custom_field_name=value~string]')
```

Ограничение из источника: условия по кастомным полям с операторами сравнения работают только на реляционных СУБД, например MS SQL или PostgreSQL. На XML-базе такой вариант работать не будет.

### MatchSome()

Проверяет попадание в набор значений.

```xquery
for $elem in events
where MatchSome($elem/type_id, ('one_time', 'real_time'))
return $elem
```

### some ... satisfies

Используется для проверки существования связанной записи:

```xquery
for $elem1 in <каталог1>
where some $elem2 in <каталог2> satisfies (<критерий>)
return $elem1
```

### null(), true(), false(), date()

Для пустого значения:

```xquery
for $elem in collaborators
where $elem/some_field = null()
return $elem
```

Для булевых значений:

```xquery
for $x in groups
where $x/is_dynamic = true()
return $x
```

```xquery
for $er in event_results
where $er/is_assist = false()
return $er
```

Для дат:

```xquery
for $elem in events
where $elem/start_date > date('2011-06-27')
return $elem
```

Источник рекомендует для новых примеров использовать ISO-подобный формат `YYYY-MM-DD` или `YYYY-MM-DDTHH:MM:SS`, если нет требования совместимости.

## Практические примеры

Сотрудники по ФИО:

```xquery
for $elem in collaborators
where contains($elem/fullname, 'Test')
return $elem
```

Сотрудник по `login`:

```xquery
for $elem in collaborators
where $elem/login = 'TestTestTest'
return $elem
```

Объект по ID:

```xquery
for $elem in collaborators
where $elem/id = 6511610587817075498
return $elem
```

Тест по коду:

```xquery
for $elem in assessments
where $elem/code = '00000017'
return $elem
```

Курс по коду:

```xquery
for $elem in courses
where $elem/code = 'OTM8'
return $elem
```

Завершенные тесты сотрудника:

```xquery
for $emp in test_learnings
where contains($emp/person_fullname, 'Иванов Иван Иванович')
return $emp
```

Незавершенные тесты сотрудника:

```xquery
for $emp in active_test_learnings
where contains($emp/person_fullname, 'Иванов Иван Иванович')
return $emp
```

Пройденные тесты среди завершенных:

```xquery
for $elem in test_learnings
where contains($elem/person_fullname, 'Иванов Иван Иванович') and $elem/state_id = 4
return $elem
```

## Практические правила

- Не считать XQuery в SP-XML полным стандартным XQuery.
- Проверять имена каталогов и полей по конкретной конфигурации.
- Для строковых значений в тексте запроса использовать одинарные кавычки, если весь запрос хранится в строке с двойными кавычками.
- Для булевых значений использовать `true()` и `false()`.
- Для пустых значений использовать `null()`.
- Для дат использовать `date()`.
- Для нескольких значений использовать `MatchSome()`.
- Для подстроки использовать `contains()`, но учитывать производительность.
- Для связей каталогов объявлять несколько переменных в `for`.

## TODO / вопросы для проверки

- TODO: нужно проверить полный список каталогов и полей для локального WebSoft HCM.
- TODO: нужно проверить производительность `contains()` на локальной XML-базе и PostgreSQL.
- TODO: нужно проверить поведение `doc-contains()` по кастомным полям на текущем учебном PostgreSQL-стенде.
- TODO: нужно проверить, поддерживает ли локальная версия все примеры `some ... satisfies`.

## Источники

- `raw/2026-06-17/xquery/spxml-xquery-query-syntax.md`
- `raw/2026-06-17/xquery/xquery-syntax.txt`
