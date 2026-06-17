---
title: "CallObjectMethodWithLock"
topic: "code-libraries/object-methods"
source_files:
  - "raw/2026-06-17/code-libraries/CallObjectMethodWithLock.txt"
status: "draft"
---

# CallObjectMethodWithLock

`CallObjectMethodWithLock( object, methodName, argsArray, lock )` работает как `CallObjectMethod(...)`, но с блокировкой через объект `Lock`.

Если другой поток вызывает функцию с тем же `lock`, выполнение в другом потоке не начнется, пока не завершится первый вызов.

TODO: нужно проверить объект `Lock` и связанные функции `GetObjectPropertyWithLock`, `SetObjectPropertyWithLock`.
