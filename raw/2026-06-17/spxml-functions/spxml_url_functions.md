---
title: "SP-XML: Работа с URL"
source: "Pasted text.txt"
status: "draft"
---

# SP-XML: Работа с URL

## Список функций

- `AbsoluteUrl()`
- `AddUrlMapping()`
- `ContentTypeToFileNameSuffix()`
- `CopyUrl()`
- `DeleteUrl()`
- `EnumerateUrls()`
- `FilePathToUrl()`
- `IsAbsoluteUrlStr()`
- `LoadUrlData()`
- `LoadUrlText()`
- `PutUrlData()`
- `PutUrlText()`
- `ReplaceUrlPathSuffix()`
- `UrlAppendPath()`
- `UrlExists()`
- `UrlFileName()`
- `UrlFileSize()`
- `UrlHost()`
- `UrlModDate()`
- `UrlOrFilePathToFilePath()`
- `UrlParam()`
- `UrlParent()`
- `UrlPath()`
- `UrlPathSuffix()`
- `UrlQuery()`
- `UrlScheme()`
- `UrlStdContentType()`
- `UrlToFilePath()`
- `UrlToOptFilePath()`
- `WebAppUrl()`

---

## AbsoluteUrl()

Преобразует заданный url в абсолютный.

Если заданный url и так является абсолютным, возвращается он же.

### Синтаксис

```spxml
AbsoluteUrl( url )
AbsoluteUrl( url, baseUrl )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | Относительный url. |
| `baseUrl` | `String` | нет | Базовый абсолютный url, относительно которого считается относительный url. Если аргумент не указан, в качестве базового url используется родительский url файла, содержащий выполняемый код. |

### Результат

`String`

### Пример

```spxml
AbsoluteUrl( 'zz/1.htm', 'x-local://data/static' )
```

Вернет:

```text
x-local://data/static/zz/1.htm
```

```spxml
AbsoluteUrl( 'zz/1.htm' )
```

Вызванный в библиотеке `x-app://rcr/rcr_lib_recruit.js`, вернет:

```text
x-app://rcr/zz/1.htm
```

---

## AddUrlMapping()

Регистрирует автоматическую подмену одного url другим.

После вызова функции при попытке любого обращения к url, являющегося дочерним по отношению к исходному базовому, будет происходить обращение к новому url, полученному путем замены исходной базовой части на новую базовую часть.

Функция как правило используется для конвертации данных из предыдущих версий программ в новую, при которой старые формы `.xmd` более не существуют и заменяются на новые.

### Синтаксис

```spxml
AddUrlMapping( baseUrl, newBaseUrl )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `baseUrl` | `String` | да | Базовый url, который нужно подменить. |
| `newBaseUrl` | `String` | да | Базовый url, на который нужно подменить. |

### Результат

`undefined`

---

## ContentTypeToFileNameSuffix()

Функция `ContentTypeToFileNameSuffix()` принимает на вход строку, содержащую MIME Content type, и возвращает рекомендуемое расширение имени файла для данного типа.

Если тип не входит во встроенный список, возвращается пустая строка.

Если в качестве аргумента уже передано расширение — строка, начинающаяся на точку, — возвращается оно же.

### Синтаксис

```spxml
ContentTypeToFileNameSuffix( contentType )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `contentType` | `String` | да | MIME Content type. |

### Пример

```spxml
UrlStdContentType( 'text/plain' )
```

Вернет:

```text
.txt
```

---

## CopyUrl()

Копирует содержимое под заданным url в новый url.

### Синтаксис

```spxml
CopyUrl( destUrl, srcUrl )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `destUrl` | `String` | да | Url, в который производится копирование. |
| `srcUrl` | `String` | да | Url, содержимое которого копируется. |

### Результат

`undefined`

---

## DeleteUrl()

Удаляет объект с заданным url.

### Синтаксис

```spxml
DeleteUrl( url )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | Url. |

### Результат

`undefined`

---

## EnumerateUrls()

Возвращает список всех существующих url, начинающихся с заданного базового url.

---

## FilePathToUrl()

Преобразует путь файловой системы в локальный url типа `file:` или `x-local:`.

### Синтаксис

```spxml
FilePathToUrl( filePath )
FilePathToUrl( filePath, baseUrl )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `filePath` | `String` | да | Путь файловой системы. |
| `baseUrl` | `String` | нет | Базовый url, к схеме которого будет приводиться путь. |

### Результат

`String`

### Пример

```spxml
FilePathToUrl( 'C:\\Temp\\1.htm' )
```

Вернет:

```text
file:///C:/Temp/1.htm
```

---

## IsAbsoluteUrlStr()

Проверяет является ли строка абсолютным URL.

Существование объекта под указанным url не проверяется.

### Синтаксис

```spxml
IsAbsoluteUrlStr( url )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | Строка с URL. |

### Результат

`Bool`

### Пример

```spxml
IsAbsoluteUrlStr( 'http://www.ya.ru/search.htm' )
```

Вернет:

```text
true
```

```spxml
IsAbsoluteUrlStr( 'search.htm' )
```

Вернет:

```text
false
```

---

## LoadUrlData()

Загружает содержимое заданного url и возвращает его в виде строки, содержащей бинарные данные.

### Синтаксис

```spxml
LoadUrlData( url )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | Url. |

### Результат

`String`

---

## LoadUrlText()

Загружает содержимое файла с заданным url с учетом наличия BOM. Если файл начинается на UTF-16 BOM, происходит конвертация из UTF-16 в текущую кодировку — UTF-8.

Если файл начинается на UTF-8 BOM, возвращается содержимое файла после BOM.

Если BOM в файле отсутствует, и не задана опция `DetectContentCharset`, происходит конвертация из однобайтовой кодировки по умолчанию, например Windows-1251, в текущую — UTF-8.

Если задана опция `DetectContentCharset`, то функция предварительно пытается определить, не содержит ли файл данные в UTF-8, и, если да, возвращает содержимое файла без изменений.

### Синтаксис

```spxml
LoadUrlText( url )
LoadUrlText( url, options )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | URL файла. |
| `options` | `Object` | нет | Стандартный объект, содержащий опции. |

### Описание

В настоящий момент поддерживается единственная опция: `DetectContentCharset` (`Bool`).

---

## PutUrlData()

Сохраняет содержимое строки, содержащей бинарные данные, в заданном url.

### Синтаксис

```spxml
PutUrlData( url, dataStr )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | Url. |
| `dataStr` | `String` | да | Строка. |

### Результат

`undefined`

---

## PutUrlText()

Сохраняет содержимое строки в файл с заданном url, с использованием UTF-8 BOM.

---

## ReplaceUrlPathSuffix()

Функция `ReplaceUrlPathSuffix()` получает на вход URI, заменяет в нем расширение имени файла и возвращает новый URI. Необходимо иметь в виду, что если будет передан относительный URI, он будет приведен к абсолютному по общим правилам.

Заменяет суффикс — расширение — имени файла в заданном url. Если заданный url имеет другой суффикс в имени файла, возвращается исходный url.

Функция не осуществляет фактического обращения к файловой системе.

### Синтаксис

```spxml
ReplaceUrlPathSuffix( url, suffix1, suffix2 )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | Url. |
| `suffix1` | `String` | да | Расширение, которое нужно заменить. |
| `suffix2` | `String` | да | Расширение, на которое нужно заменить. |

### Результат

`String`

### Пример

```spxml
ReplaceUrlPathSuffix( 'http://news.websoft.ru/tree.html?query', 'html', 'asp' )
```

Вернет:

```text
http://news.websoft.ru/tree.asp?query
```

---

## UrlAppendPath()

Добавляет фрагмент пути к заданному url.

### Синтаксис

```spxml
UrlAppendPath( url, addPath )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | Url. |
| `addPath` | `String` | да | Добавляемый путь. |

### Результат

`String`

### Пример

```spxml
UrlAppendPath( 'http://www.lin.ru/service', 'z/1.htm' )
```

Вернет:

```text
http://www.lin.ru/service/z/1.htm
```

---

## UrlExists()

Проверяет, что файл с заданным url существует. Возвращает `true` или `false`.

Для файлов, располагающихся в базе данных, функция может вызывать загрузку полного содержимого файла.

---

## UrlFileName()

Извлекает имя файла из заданного url.

### Синтаксис

```spxml
UrlFileName( url )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | Url. |

### Результат

`String`

### Пример

```spxml
UrlHost( 'http://news.websoft.ru/db/kb/0939DD37D1C5F9B8C3257403003E8F4F/tree.html?query=xxx' )
```

Вернет:

```text
tree.html
```

---

## UrlFileSize()

Определяет размер файла в байтах по локальному url, переданному в качестве аргумента.

### Синтаксис

```spxml
UrlFileSize( url )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | Локальный url типа `file:` или `x-local:`. |

### Результат

`Integer`

---

## UrlHost()

Возвращает хост из переданного в качестве аргумента URL.

### Синтаксис

```spxml
UrlHost( url )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | Url. |

### Результат

`String`

### Пример

```spxml
UrlHost( 'http://news.websoft.ru/db/kb/0939DD37D1C5F9B8C3257403003E8F4F/tree.html?query=xxx' )
```

Вернет:

```text
news.websoft.ru
```

---

## UrlModDate()

Возвращает дату изменения файла, находящегося по локальному пути типа `file:` или `x-local:`.

### Синтаксис

```spxml
UrlModDate( url )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | Url. |

### Результат

`Date`

---

## UrlOrFilePathToFilePath()

Функция `UrlOrFilePathToFilePath()` проверяет, является ли строка урлом либо путем в файловой системе. В первом случае функция преобразовывает переданный url вида `file:`, `x-app:` или `x-local:` в путь файловой системы. Во втором случае вернет переданный путь в исходном виде.

### Синтаксис

```spxml
UrlOrFilePathToFilePath( str )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `str` | `String` | да | Url файла либо путь к файлу. |

### Пример

```spxml
UrlOrFilePathToFilePath( 'file:///d:/work/Temp.rar' )
```

Вернет:

```text
d:\work\Temp.rar
```

---

## UrlParam()

Извлекает из url, переданного в качестве аргумента, строку запроса в исходном виде.

### Синтаксис

```spxml
UrlParam( url )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | Url. |

### Результат

`String`

### Пример

```spxml
UrlHost( 'http://news.websoft.ru/db/kb/0939DD37D1C5F9B8C3257403003E8F4F/tree.html?query=xxx' )
```

Вернет:

```text
query=xxx
```

---

## UrlParent()

Извлекает url родительской директории из заданного url.

### Синтаксис

```spxml
UrlParent( url )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | Url. |

### Результат

`String`

### Пример

```spxml
UrlParent( 'http://news.websoft.ru/db/kb/0939DD37D1C5F9B8C3257403003E8F4F/tree.html?query=xxx' )
```

Вернет:

```text
http://news.websoft.ru/db/kb/0939DD37D1C5F9B8C3257403003E8F4F/
```

---

## UrlPath()

Извлекает из URL, переданного в качестве аргумента, путь.

### Синтаксис

```spxml
UrlPath( url )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | Url. |

### Результат

`String`

### Пример

```spxml
UrlPath( 'http://news.websoft.ru/db/kb/0939DD37D1C5F9B8C3257403003E8F4F/tree.html' )
```

Вернет:

```text
/db/kb/0939DD37D1C5F9B8C3257403003E8F4F/tree.html
```

---

## UrlPathSuffix()

Возвращает расширение файла, указанного через url.

### Синтаксис

```spxml
UrlPathSuffix( url )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | Url. |

### Результат

`String`

### Пример

```spxml
UrlPathSuffix( 'http://news.websoft.ru/db/kb/0939DD37D1C5F9B8C3257403003E8F4F/tree.html' )
```

Вернет:

```text
.html
```

---

## UrlQuery()

Извлекает из URL, переданного в качестве аргумента, параметры запроса с разбивкой их на пары `имя - значение`.

### Синтаксис

```spxml
UrlQuery( url )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | Url. |

### Результат

`Object (JavaScript)`

### Пример

```spxml
obj = UrlQuery( 'http://news.websoft.ru/en?x=1&y=2&z=3' );
alert( obj.x );
alert( obj.y );
```

---

## UrlScheme()

Возвращает схему URL: `file`, `http`, `mailto`, `ftp`, `x-local`.

В старых версиях платформы функция была также доступна по имени `UrlSchema()`.

### Синтаксис

```spxml
UrlScheme( url )
UrlSchema( url )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | Url. |

### Результат

`String`

### Пример

```spxml
UrlSchema( 'http://docs.datex.ru/' )
```

Вернет:

```text
http
```

---

## UrlStdContentType()

Функция `UrlStdContentType()` принимает на вход строку, содержащую абсолютный или относительный uri, и возвращает MIME Content type, определяемый по расширении имени файла в этом uri.

Если расширение не входит во встроенный список известных расширений, возвращается пустая строка.

### Синтаксис

```spxml
UrlStdContentType( url )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | URI, URI path, либо имя файла. |

### Пример

```spxml
UrlStdContentType( 'File.txt' )
```

Вернет:

```text
text/plain
```

---

## UrlToFilePath()

Преобразует локальный url типа `file:`, `x-app:` или `x-local:` в путь файловой системы. Если преобразование совершить не удалось, например передан url другого типа или база располагается не в локальной файловой системе, функция завершается с ошибкой.

### Синтаксис

```spxml
UrlToFilePath( url )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | Url. |

### Пример

```spxml
UrlToFilePath( 'file:///d:/work/Temp.rar' )
```

Вернет:

```text
d:\work\Temp.rar
```

---

## UrlToOptFilePath()

Преобразует локальный url типа `file:`, `x-app:` или `x-local:` в путь файловой системы. Если преобразование совершить не удалось, например передан url другого типа или база располагается не в локальной файловой системе, функция возвращает `undefined`.

### Синтаксис

```spxml
UrlToOptFilePath( url )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | Url. |

### Пример

```spxml
UrlToOptFilePath( 'file:///d:/work/Temp.rar' )
```

Вернет:

```text
d:\work\Temp.rar
```

---

## WebAppUrl()

Преобразует заданный url схемы `x-app` в url, пригодный для использования во встроенном браузере — элемент `HYPER`.

Для десктоп-версии осуществляется преобразование в url схемы `file`, а для веб-версии — в специальный серверный запрос.

### Синтаксис

```spxml
WebAppUrl( url )
```

### Аргументы

| Аргумент | Тип | Обязательный | Описание |
|---|---|---:|---|
| `url` | `String` | да | Url. |

### Результат

`String`
