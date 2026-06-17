---
title: "GetObjectProperty"
topic: "code-libraries/object-properties"
source_files:
  - "raw/2026-06-17/code-libraries/GetObjectProperty.txt"
status: "draft"
---

# GetObjectProperty

`GetObjectProperty( object, propName )` получает значение свойства абстрактного объекта по имени динамическим способом.

По источнику:

```text
GetObjectProperty( object, 'xxx' )
```

эквивалентно:

```text
object.xxx
```

Для стандартного объекта вызов эквивалентен `object[propName]`.

При отсутствующем свойстве возможны исключение или возврат `undefined`, в зависимости от типа объекта.
