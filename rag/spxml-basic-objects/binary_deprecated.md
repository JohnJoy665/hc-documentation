---
title: "Binary считается устаревшим"
topic: "spxml-basic-objects/binary"
source_files:
  - "raw/2026-06-17/spxml-basic-objects/spxml-basic-object-binary.md"
status: "draft"
---

# Binary считается устаревшим

`Binary` хранит бинарные данные и исторически использовался как буфер для больших результатов.

Источник отмечает, что объект считается устаревшим. Для нового кода при буферизованной записи большого результата нужно проверить `WriteBuffStream`.

Методы: `AssignBinary`, `AssignStr`, `AppendBinary`, `AppendStr`, `GetStr`, `LoadFromUrl`, `PutToUrl`.
