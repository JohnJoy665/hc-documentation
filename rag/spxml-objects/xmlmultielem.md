---
title: "XmlMultiElem"
topic: "spxml-objects/xml-multi-elem"
source_files:
  - "raw/2026-06-18/xml-objects/xml-doc-objects-xmlmultielem-ai-friendly.md"
status: "draft"
---

# XmlMultiElem

`XmlMultiElem` - виртуальный массив одноименных XML-элементов, описанных в XMD-форме с `MULTIPLE="1"`.

Он не является физическим XML-узлом. Это обертка над набором фактических дочерних `XmlElem` одного родителя.

Основные методы:

- `Add()` - добавляет новый множественный элемент и возвращает `XmlElem`.
- `ByValueExists(value)` - проверяет наличие значения.
- `ObtainByValue(value)` - находит или создает элемент по значению.
- `SetValues(values)` - полностью заменяет список значений.
- `Clear()` - удаляет все соответствующие множественные элементы.
- `DeleteAll()` - устаревший синоним `Clear()`.
- `DeleteByValue(value)` - удаляет первый элемент с заданным значением; если не найден, возникает ошибка.

Безопасное удаление значения:

```js
if ( candidate.profession_id.ByValueExists( 2054 ) )
{
    candidate.profession_id.DeleteByValue( 2054 );
}
```
