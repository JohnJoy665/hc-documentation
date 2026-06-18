---
title: "Object.GetOptProperty"
topic: "spxml-basic-objects/object"
source_files:
  - "raw/2026-06-17/spxml-basic-objects/spxml-basic-object-object.md"
status: "draft"
---

# Object.GetOptProperty

`GetOptProperty(propName[, defaultVal])` безопасно читает свойство объекта.

Если свойства нет:

- без второго аргумента возвращает `undefined`;
- со вторым аргументом возвращает `defaultVal`.

Использовать для пользовательских, конфигурационных и необязательных параметров.
