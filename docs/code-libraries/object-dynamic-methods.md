---
title: "Динамический доступ к методам и свойствам объектов"
topic: "code-libraries/object-methods"
source_files:
  - "raw/2026-06-17/code-libraries/CallObjectMethod.txt"
  - "raw/2026-06-17/code-libraries/CallObjectMethodWithLock.txt"
  - "raw/2026-06-17/code-libraries/GetClassObjectPropertyNames.txt"
  - "raw/2026-06-17/code-libraries/GetObjectProperty.txt"
  - "raw/2026-06-17/spxml-functions/spxml_object_functions.md"
status: "draft"
---

# Динамический доступ к методам и свойствам объектов

## Кратко

PDF с docs.datex.ru описывают несколько функций SP-XML для динамической работы с объектами:

- `CallObjectMethod(...)`
- `CallObjectMethodWithLock(...)`
- `GetClassObjectPropertyNames(...)`
- `GetObjectProperty(...)`
- `GetObjectPropertyOrSelf(...)`
- `GetObjectPropertyWithLock(...)`
- `GetOptObjectProperty(...)`
- `SetObjectProperty(...)`
- `SetObjectPropertyWithLock(...)`

Эти функции относятся к платформенной среде SP-XML / WebTutor и не должны автоматически трактоваться как методы внешних ECMAScript-сред.

## CallObjectMethod

Назначение из источника: вызвать метод абстрактного объекта по имени динамическим способом.

Синтаксис:

```text
CallObjectMethod( object, methodName, argsArray )
```

Эквивалентность из источника:

```text
CallObjectMethod( object, 'Test', [arg1, arg2] )
```

эквивалентно:

```text
object.Test( arg1, arg2 )
```

Аргументы:

- `object` - объект произвольного типа;
- `methodName` - имя метода;
- `argsArray` - стандартный массив аргументов.

## CallObjectMethodWithLock

Назначение: работает аналогично `CallObjectMethod(...)`, но с блокировкой доступа через переданный `lock`.

Синтаксис:

```text
CallObjectMethodWithLock( object, methodName, argsArray, lock )
```

Аргументы:

- `object` - объект произвольного типа;
- `methodName` - имя метода;
- `argsArray` - стандартный массив аргументов;
- `lock` - объект типа `Lock`.

Источник поясняет: если во время работы функции из другого потока будет вызвана другая функция с тем же `lock`, выполнение в другом потоке не начнется, пока не завершится вызов в первом потоке.

## GetClassObjectPropertyNames

Назначение: возвращает список имен всех свойств объектов класса.

Синтаксис:

```text
GetClassObjectPropertyNames( object )
```

Источник уточняет: возвращается полный список имен, описанных в декларации класса, независимо от того, было ли конкретное свойство установлено в данном объекте.

## GetObjectProperty

Назначение: получить значение свойства абстрактного объекта по имени динамическим способом.

Синтаксис:

```text
GetObjectProperty( object, propName )
```

Эквивалентность из источника:

```text
GetObjectProperty( object, 'xxx' )
```

эквивалентно:

```text
object.xxx
```

Для стандартного объекта:

```text
GetObjectProperty( object, propName )
```

эквивалентно:

```text
object[propName]
```

При попытке получить отсутствующее свойство в зависимости от типа объекта возможно исключение или возврат `undefined`.

## GetObjectPropertyOrSelf

Назначение из источника: функция эквивалентна `GetObjectProperty()`, кроме случая, когда `propName` равно `"This"`. В этом случае возвращается ссылка на переданный объект.

Синтаксис:

```text
GetObjectPropertyOrSelf( object, propName )
```

## GetOptObjectProperty

Назначение из источника: функция эквивалентна `GetObjectProperty()`, но если свойство отсутствует, возвращает `undefined`.

Синтаксис:

```text
GetOptObjectProperty( object, propName )
```

## SetObjectProperty

Назначение из источника: установить значение свойства абстрактного объекта по имени динамическим способом.

Синтаксис:

```text
SetObjectProperty( object, propName, propValue )
```

Эквивалентность из источника:

```text
SetObjectProperty( object, 'xxx', value )
```

эквивалентно:

```text
object.xxx = value
```

Для стандартного объекта:

```text
SetObjectProperty( object, propName, value )
```

эквивалентно:

```text
object[propName] = value
```

В зависимости от типа объекта при попытке установить отсутствующее свойство возможно исключение.

## GetObjectPropertyWithLock и SetObjectPropertyWithLock

`GetObjectPropertyWithLock()` и `SetObjectPropertyWithLock()` работают аналогично чтению/записи свойства, но с блокировкой доступа через переданный `lock`.

Источник для `SetObjectPropertyWithLock()` указывает синтаксис:

```text
SetObjectPropertyWithLock( object, propName, argsArray, lock )
```

TODO: нужно проверить, почему в синтаксисе указан `argsArray`, а не `propValue`, и не является ли это ошибкой источника.

## TODO / вопросы для проверки

- TODO: нужно собрать примеры безопасного применения этих функций в серверном коде WebTutor.
- TODO: нужно проверить функции `GetObjectPropertyWithLock`, `SetObjectPropertyWithLock` и объект `Lock`.
- TODO: нужно проверить, какие типы объектов в WebTutor могут возвращать исключение при отсутствующем свойстве.
- TODO: нужно проверить синтаксис `SetObjectPropertyWithLock()` по официальному источнику или стенду.

## Источники

- `raw/2026-06-17/code-libraries/CallObjectMethod.txt`
- `raw/2026-06-17/code-libraries/CallObjectMethodWithLock.txt`
- `raw/2026-06-17/code-libraries/GetClassObjectPropertyNames.txt`
- `raw/2026-06-17/code-libraries/GetObjectProperty.txt`
- `raw/2026-06-17/spxml-functions/spxml_object_functions.md`
