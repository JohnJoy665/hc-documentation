---
title: "CallObjectMethod"
topic: "code-libraries/object-methods"
source_files:
  - "raw/2026-06-17/code-libraries/CallObjectMethod.txt"
status: "draft"
---

# CallObjectMethod

`CallObjectMethod( object, methodName, argsArray )` вызывает метод абстрактного объекта по имени динамическим способом.

По источнику:

```text
CallObjectMethod( object, 'Test', [arg1, arg2] )
```

эквивалентно:

```text
object.Test( arg1, arg2 )
```
