---
title: "ArrayDirect и ArraySelectAll"
topic: "spxml-functions/array"
source_files:
  - "raw/2026-06-17/spxml-functions/spxml_array_functions.md"
status: "draft"
---

# ArrayDirect и ArraySelectAll

В SP-XML массив может быть абстрактным перечислителем или результатом XQuery, а не индексируемым массивом.

`ArrayDirect()` гарантирует прямой доступ по индексу. Если исходный массив уже индексируемый, возвращает его же.

`ArraySelectAll()` всегда возвращает стандартный массив `Array` со всеми элементами исходного массива.

Использовать перед многократной обработкой, случайным доступом или удалением элементов в цикле.
