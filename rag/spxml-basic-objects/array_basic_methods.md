---
title: "Array: базовые методы"
topic: "spxml-basic-objects/array"
source_files:
  - "raw/2026-06-17/spxml-basic-objects/spxml-basic-object-array.md"
status: "draft"
---

# Array: базовые методы

`Array` хранит упорядоченный набор элементов.

Ключевые методы:

- `push(...)` - добавляет элементы в конец и возвращает новую длину.
- `join(separator)` - соединяет элементы в строку.
- `indexOf(value[, startIndex])` - возвращает индекс или `-1`.
- `splice(startIndex, count)` - удаляет элементы.
- `AppendUniqueValue(value)` - добавляет только новое значение.
- `ObtainByValue(value)` - возвращает индекс найденного или добавленного значения.

Методы `FindIndexBySortedValue()` и `ObtainBySortedValue()` использовать только для отсортированных массивов.
