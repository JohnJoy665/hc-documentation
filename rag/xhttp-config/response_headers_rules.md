---
title: "xhttp_config.json ResponseHeadersRules"
topic: "xhttp-config/headers"
source_files:
  - "raw/2026-06-17/pdf/xhttp-config-json.txt"
status: "draft"
---

# xhttp_config.json ResponseHeadersRules

`ResponseHeadersRules` позволяет удалить или добавить HTTP-заголовки в ответах xHTTP.

Пример удаления:

```json
"ResponseHeadersRules": [
  { "Host": "*", "Remove": ["Server"] }
]
```

Пример добавления:

```json
"ResponseHeadersRules": [
  { "Host": "*", "Add": {"<Имя_заголовка>": "<Значение_заголовка>"} }
]
```

TODO: нужно проверить валидность конкретных JSON-примеров после ручной сверки с PDF.
