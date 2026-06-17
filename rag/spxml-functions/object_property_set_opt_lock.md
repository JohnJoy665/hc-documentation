---
title: "GetOptObjectProperty и SetObjectProperty"
topic: "spxml-functions/object"
source_files:
  - "raw/2026-06-17/spxml-functions/spxml_object_functions.md"
status: "draft"
---

# GetOptObjectProperty и SetObjectProperty

`GetOptObjectProperty(object, propName)` аналогична `GetObjectProperty()`, но при отсутствии свойства возвращает `undefined`.

`SetObjectProperty(object, propName, propValue)` устанавливает свойство по имени динамически.

Для стандартного объекта:

```text
SetObjectProperty( object, propName, value )
```

эквивалентно:

```text
object[propName] = value
```

`GetObjectPropertyWithLock()` и `SetObjectPropertyWithLock()` используют объект `Lock`.
