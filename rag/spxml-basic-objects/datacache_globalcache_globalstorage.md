---
title: "DataCache, GlobalCache, GlobalStorage"
topic: "spxml-basic-objects/datacache"
source_files:
  - "raw/2026-06-17/spxml-basic-objects/spxml-basic-objects-class-cache-error-methodreference.md"
status: "draft"
---

# DataCache, GlobalCache, GlobalStorage

`DataCache` - thread-safe key-value-хранилище.

Глобальные объекты:

- `GlobalCache` - общий кэш приложения;
- `GlobalStorage` - хранилище, сохраняющее значения между запусками приложения.

Методы:

- `SetEntry(keyName, value[, options])`
- `GetEntry(keyName)` - значение или `undefined`;
- `DeleteEntry(keyName)`

Опция `TTL` задает время жизни записи в секундах.
