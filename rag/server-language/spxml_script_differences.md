---
title: "SP-XML Script отличается от ECMAScript-сред"
topic: "server-language/spxml-script"
source_files:
  - "raw/2026-06-17/server-language/spxml-script-vs-javascript.txt"
status: "draft"
---

# SP-XML Script отличается от ECMAScript-сред

Источник docs.datex.ru прямо указывает, что SP-XML Script является отдельным языком, хотя синтаксически похож на ECMAScript-семейство.

Важные отличия:

- строка и дата - скалярные значения;
- `integer` и `real` - разные типы;
- строки бинарно-совместимы;
- `StrLen()` возвращает длину в байтах;
- `StrCharCount()` возвращает длину в символах;
- `undefined`, `null`, `''`, `0`, `false` - разные значения;
- `for-in` по абстрактным массивам работает как foreach.

Серверный код WebTutor нельзя автоматически писать как код для Node.js или браузера.
