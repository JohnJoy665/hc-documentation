---
title: "OpenDoc, FetchDoc и кэш XML-документов"
topic: "spxml-functions/xml-documents"
source_files:
  - "raw/2026-06-17/spxml-functions/spxml_xml_document_functions.md"
  - "raw/2026-06-17/spxml-functions/spxml_application_structure.md"
status: "draft"
---

# OpenDoc, FetchDoc и кэш XML-документов

`OpenDoc()` открывает XML-документ по URL.

`FetchDoc()` открывает XML-документ и помещает его в кэш документов. Если документ уже в кэше, возвращается кэшированный документ.

Для чтения из кэша:

- `GetCachedDoc(url)` - ошибка, если документа нет;
- `GetOptCachedDoc(url)` - `undefined`, если документа нет.

TODO: нужно проверить правила сохранения и инвалидации кэша на реальном стенде.
