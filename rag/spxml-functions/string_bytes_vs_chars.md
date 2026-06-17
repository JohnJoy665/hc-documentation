---
title: "SP-XML строки: байты и символы"
topic: "spxml-functions/string"
source_files:
  - "raw/2026-06-17/spxml-functions/spxml_string_functions.md"
status: "draft"
---

# SP-XML строки: байты и символы

В SP-XML часть строковых функций работает с байтами, часть - с символами.

- `StrLen()` - длина в байтах.
- `StrCharCount()` - количество символов.
- `StrLeftRange()` - фрагмент по длине в байтах.
- `StrLeftCharRange()` - фрагмент по длине в символах.
- `StrOptSubStrPos()` - позиция подстроки в байтах.
- `StrCharRangePos()` - позиция/диапазон в символах.

Для строк с не-ASCII символами нельзя смешивать байтовые и символьные позиции без проверки.
