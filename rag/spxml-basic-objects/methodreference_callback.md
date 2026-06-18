---
title: "MethodReference"
topic: "spxml-basic-objects/methodreference"
source_files:
  - "raw/2026-06-17/spxml-basic-objects/spxml-basic-objects-class-cache-error-methodreference.md"
status: "draft"
---

# MethodReference

`MethodReference` - ссылка на метод библиотеки, обычно для обработчиков или callback-методов.

Пример:

```text
Application.OnClientMessage = MethodReference( lib_recruit, 'OnClientMessage' );
```

Для явного вызова используется:

```text
handler.Call( [msg] );
```

`MethodReference(...)` создает ссылку, а не вызывает метод сразу.
