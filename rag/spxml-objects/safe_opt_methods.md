---
title: "Безопасные Opt-методы XML-элементов"
topic: "spxml-objects/xml-elem/safe-access"
source_files:
  - "raw/2026-06-18/xml-objects/xml-doc-objects-xmlelem-ai-friendly.md"
status: "draft"
---

# Безопасные Opt-методы XML-элементов

Для допустимого отсутствия XML-элемента, пути, атрибута или внешней ссылки источник рекомендует использовать безопасные методы с `Opt` или методы `...Exists`.

Примеры:

- `OptChild(name)` возвращает дочерний элемент или `undefined`.
- `GetOptChildByKey(key)` возвращает дочерний элемент по ключу или `undefined`.
- `EvalOptPath(path)` возвращает элемент по пути или `undefined`.
- `OptForeignElem` возвращает связанный внешний элемент или `undefined`.
- `DeleteOptChildByKey(key)` безопасно ничего не делает, если элемент не найден.
- `DeleteOptAttr(name)` безопасно ничего не делает, если атрибут отсутствует.

Методы без `Opt`, например `GetChildByKey()` или `DeleteChildByKey()`, могут возвращать ошибку при отсутствии элемента.
