---
title: "SP-XML даты: разбор и форматирование"
topic: "spxml-functions/date"
source_files:
  - "raw/2026-06-17/spxml-functions/spxml_date_functions.md"
status: "draft"
---

# SP-XML даты: разбор и форматирование

Для дат в источнике перечислены:

- `Date()`, `OptDate()`
- `ParseDate()`, `ParseMimeDate()`
- `IsValidDate()`
- `DateDiff()`, `DateOffset()`
- `StrDate()`, `StrLongDate()`, `StrShortDate()`, `StrTime()`
- `StrXmlDate()`, `StrMimeDate()`

TODO: нужно проверить спорные места источника: безопасность `Date()` для пользовательского ввода, результат `StrMimeDate()`, потерю временной зоны при преобразованиях.
