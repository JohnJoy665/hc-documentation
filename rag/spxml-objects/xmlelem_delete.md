---
title: "Удаление XML-элементов через XmlElem"
topic: "spxml-objects/xml-elem/delete"
source_files:
  - "raw/2026-06-18/xml-objects/xml-doc-objects-xmlelem-ai-friendly.md"
status: "draft"
---

# Удаление XML-элементов через XmlElem

Новый источник подтверждает методы удаления внутри XML-документа:

- `Delete()` - удаляет текущий элемент.
- `DeleteChildren()` - удаляет дочерние элементы по условию или все.
- `DeleteChildByKey()` - удаляет дочерний элемент по ключу; если не найден, возникает ошибка.
- `DeleteOptChildByKey()` - безопасно удаляет дочерний элемент по ключу; если не найден, ничего не делает.
- `DeleteChildrenByValue()` - удаляет дочерние элементы по значению.
- `Clear()` - очищает значение и дочерние элементы.

Пример безопасного удаления по ключу:

```js
list.DeleteOptChildByKey( keyValue );
```

Для динамических атрибутов: `DeleteAttr()` удаляет атрибут с ошибкой при отсутствии, `DeleteOptAttr()` безопасно ничего не делает, если атрибута нет.
