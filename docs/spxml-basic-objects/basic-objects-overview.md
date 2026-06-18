---
title: "SP-XML: базовые объекты языка"
topic: "spxml-basic-objects"
source_files:
  - "raw/2026-06-17/spxml-basic-objects/spxml-basic-object-array.md"
  - "raw/2026-06-17/spxml-basic-objects/spxml-basic-object-binary.md"
  - "raw/2026-06-17/spxml-basic-objects/spxml-basic-object-object.md"
  - "raw/2026-06-17/spxml-basic-objects/spxml-basic-object-string.md"
  - "raw/2026-06-17/spxml-basic-objects/spxml-basic-objects-class-cache-error-methodreference.md"
status: "draft"
---

# SP-XML: базовые объекты языка

## Кратко

Материалы описывают базовые объекты SP-XML, которые встречаются во внутреннем серверном языке WebTutor:

- `Array`
- `Binary`
- `ClassObject`
- `ClassObjectsArray`
- `DataCache`
- `Error`
- `MethodReference`
- `Object`
- `String`

Синтаксис части методов похож на ECMAScript-среды, но поведение нужно брать из документации SP-XML / WebTutor, а не переносить автоматически из внешних сред.

## Array

`Array` - базовый объект для хранения упорядоченного набора элементов.

Свойство:

- `length` - количество элементов массива.

Методы из источника:

- `AppendUniqueValue(value)` - добавляет значение, если его еще нет; возвращает `true`, если значение добавлено, и `false`, если уже было.
- `ByValueExists(value)` - проверяет наличие значения.
- `FindIndexBySortedValue(value[, startIndex])` - ищет значение в отсортированном массиве и возвращает индекс или `-1`.
- `indexOf(value[, startIndex])` - ищет значение в массиве и возвращает индекс или `-1`.
- `ObtainBySortedValue(value)` - ищет или вставляет значение в отсортированный массив с сохранением сортировки.
- `ObtainByValue(value)` - ищет или добавляет значение в массив и возвращает индекс.
- `join([separator])` - соединяет элементы в строку.
- `push(arg1[, arg2, ...])` - добавляет элементы в конец и возвращает новую длину.
- `splice(arg1, arg2)` - удаляет `arg2` элементов начиная с индекса `arg1`.

Важно: методы с `SortedValue` допустимы только для массива, отсортированного по возрастанию.

## Object

`Object` - динамический объект-контейнер для именованных свойств.

Создание:

```text
a = new Object;
```

Методы:

- `HasProperty(propName)` - проверяет наличие свойства.
- `GetProperty(propName)` - возвращает обязательное свойство; при отсутствии вызывает ошибку.
- `GetOptProperty(propName[, defaultVal])` - безопасно читает необязательное свойство; при отсутствии возвращает `undefined` или `defaultVal`.
- `SetProperty(propName, propVal)` - устанавливает или добавляет свойство.
- `AddProperty(propName, propVal)` - добавляет свойство.

Практическое правило: для пользовательских, конфигурационных и необязательных параметров предпочтителен `GetOptProperty()`.

## String

`String` - базовый объект для строковых данных.

В источнике `String.length` описан как длина строки в байтах. Это важно для UTF-8 и других многобайтовых кодировок.

Методы:

- `Allocate(newLen)`
- `charAt(index)`
- `charCodeAt(index)`
- `fromCharCode(code)`
- `indexOf(subString[, startPos])`
- `lastIndexOf(substring[, startindex])`
- `slice(start[, end])`
- `split(separator)`
- `substr(start[, length])`
- `toLowerCase()`
- `ToWinLineBreaks()`

Для нового кода источник рекомендует аналоги через встроенные функции:

- `String.length` -> `StrLen()`
- `String.slice()` -> `StrRangePos()`, `StrLeftRange()`
- `String.substr()` -> `StrRightRangePos()`
- `String.toLowerCase()` -> `StrLowerCase()`

TODO: нужно проверить полное описание `ToWinLineBreaks()`, потому что в исходном фрагменте указано только имя.

## Binary

`Binary` - объект для хранения бинарных данных, в том числе большого размера.

Источник отмечает, что `Binary` считается устаревшим объектом. Для новой разработки при буферизованной записи большого результата нужно проверить возможность использования `WriteBuffStream`.

Свойства:

- `Size`
- `WriteStream`

Методы:

- `AssignBinary(obj)`
- `AssignStr(str)`
- `AppendBinary(obj)`
- `AppendStr(str)`
- `GetStr()`
- `LoadFromUrl(url)`
- `PutToUrl(url)`

## ClassObject и ClassObjectsArray

`ClassObject` - базовый объект для объектов конкретного класса. На практике свойства и методы зависят от конкретного класса платформы.

Пример из источника:

```text
item = new ui2_client.card_view_item;
```

`ClassObjectsArray` - типизированный массив объектов заданного класса.

Пример:

```text
items = new ClassObjectsArray( ui2_client.card_view_item );
```

Нельзя автоматически заменять `ClassObjectsArray` на `Array`, если API ожидает типизированную коллекцию.

## DataCache

`DataCache` - thread-safe key-value-хранилище с быстрым поиском по ключу и TTL.

Глобальные объекты:

- `GlobalCache` - общий временный кэш приложения;
- `GlobalStorage` - хранилище, сохраняющее значения между запусками приложения.

Методы:

- `DeleteEntry(keyName)`
- `GetEntry(keyName)` - возвращает значение или `undefined`;
- `SetEntry(keyName, value[, options])`

Опция:

- `TTL` - время жизни записи в секундах.

## Error

`Error` - объект ошибки или исключения.

Источник указывает, что формы эквивалентны:

```text
throw 'Some error';
throw new Error( 'Some error' );
```

Для диагностируемого серверного кода лучше давать понятные сообщения.

## MethodReference

`MethodReference` - ссылка на метод с заданным именем в заданной библиотеке.

Пример:

```text
Application.OnClientMessage = MethodReference( lib_recruit, 'OnClientMessage' );
```

Для явного вызова:

```text
handler.Call( [msg] );
```

Важно: `MethodReference(...)` создает ссылку на метод, а не вызывает его сразу.

## TODO / вопросы для проверки

- TODO: нужно проверить полный список базовых объектов `PlainSeq`, `SafeArray`, `Stream`, потому что в этом наборе материалов они только перечислены.
- TODO: нужно проверить `WriteBuffStream` как актуальную замену `Binary`.
- TODO: нужно проверить сигнатуру `ClassObjectsArray.Add()`.
- TODO: нужно проверить возвращаемое значение `MethodReference.Call()`.
- TODO: нужно проверить `ToWinLineBreaks()` по первичному источнику.

## Источники

- `raw/2026-06-17/spxml-basic-objects/`
