# Функции для работы с объектами

## `GetObjectPropertyOrSelf()`

Функция `GetObjectPropertyOrSelf()` эквивалентна `GetObjectProperty()` за одним исключением: если в качестве `propName` передано имя `"This"`, функция вернет ссылку на переданный объект.

### Синтаксис

```txt
GetObjectPropertyOrSelf( object, propName )
```

### Аргументы

| Аргумент | Тип | Описание |
|---|---|---|
| `object` | `object` | Объект произвольного типа |
| `propName` | `string` | Имя свойства либо `"This"` |

---

## `GetObjectPropertyWithLock()`

Функция `GetObjectPropertyWithLock()` работает аналогично функции `GetObjectProperty()`, но с блокировкой доступа через переданный `lock`.

Если во время работы функции из другого потока будет вызвана другая функция с этим же `lock`, выполнение в другом потоке не начнется, пока не завершится вызов в первом потоке.

### Синтаксис

```txt
GetObjectPropertyWithLock( object, propName, lock )
```

### Аргументы

| Аргумент | Тип | Описание |
|---|---|---|
| `object` | `object` | Объект произвольного типа |
| `propName` | `string` | Имя свойства |
| `lock` | `object` | Объект типа `Lock` |

---

## `GetOptObjectProperty()`

Функция `GetOptObjectProperty()` эквивалентна `GetObjectProperty()` за одним исключением: если в объекте отсутствует данное свойство, функция возвращает `undefined`.

### Синтаксис

```txt
GetOptObjectProperty( object, propName )
```

### Аргументы

| Аргумент | Тип | Описание |
|---|---|---|
| `object` | `object` | Объект произвольного типа |
| `propName` | `string` | Имя свойства |

---

## `SetObjectProperty()`

Функция `SetObjectProperty()` позволяет установить значение свойства абстрактного объекта по имени динамическим способом.

Вызов:

```txt
SetObjectProperty( object, 'xxx', value )
```

эквивалентен выражению:

```txt
object.xxx = value
```

Для стандартного объекта вызов:

```txt
SetObjectProperty( object, propName, value )
```

эквивалентен вызову:

```txt
object[propName] = value
```

В зависимости от типа объекта при попытке установить свойство, которое отсутствует в объекте, возможно срабатывание исключения.

### Синтаксис

```txt
SetObjectProperty( object, propName, propValue )
```

### Аргументы

| Аргумент | Тип | Описание |
|---|---|---|
| `object` | `object` | Объект произвольного типа |
| `propName` | `string` | Имя свойства |
| `propValue` |  | Значение свойства |

---

## `SetObjectPropertyWithLock()`

Функция `SetObjectPropertyWithLock()` работает аналогично функции `SetObjectProperty()`, но с блокировкой доступа через переданный `lock`.

Если во время работы функции из другого потока будет вызвана другая функция с этим же `lock`, выполнение в другом потоке не начнется, пока не завершится вызов в первом потоке.

### Синтаксис

```txt
SetObjectPropertyWithLock( object, propName, argsArray, lock )
```

### Аргументы

| Аргумент | Тип | Описание |
|---|---|---|
| `object` | `object` | Объект произвольного типа |
| `propName` | `string` | Имя свойства |
| `argsArray` | `object` | Стандартный массив аргументов |
| `lock` | `object` | Объект типа `Lock` |

### Смотри также

- `Lock`
