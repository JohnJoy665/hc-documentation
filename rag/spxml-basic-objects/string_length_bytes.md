---
title: "String.length возвращает байты"
topic: "spxml-basic-objects/string"
source_files:
  - "raw/2026-06-17/spxml-basic-objects/spxml-basic-object-string.md"
status: "draft"
---

# String.length возвращает байты

В источнике по `String` свойство `length` описано как длина строки в байтах.

Для строк с не-ASCII символами это не равно количеству символов.

Для нового кода источник рекомендует использовать встроенные функции SP-XML:

- `StrLen()` вместо `String.length`;
- `StrRangePos()` / `StrLeftRange()` вместо `slice()`;
- `StrRightRangePos()` вместо `substr()`;
- `StrLowerCase()` вместо `toLowerCase()`.
