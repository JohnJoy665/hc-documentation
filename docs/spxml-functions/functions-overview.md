---
title: "SP-XML: обзор встроенных функций"
topic: "spxml-functions/overview"
source_files:
  - "raw/2026-06-17/spxml-functions/spxml_array_functions.md"
  - "raw/2026-06-17/spxml-functions/spxml_code_execution_functions.md"
  - "raw/2026-06-17/spxml-functions/spxml_date_functions.md"
  - "raw/2026-06-17/spxml-functions/spxml_filesystem_functions.md"
  - "raw/2026-06-17/spxml-functions/spxml_format_encoding_conversion.md"
  - "raw/2026-06-17/spxml-functions/spxml_html_functions.md"
  - "raw/2026-06-17/spxml-functions/spxml_misc_functions.md"
  - "raw/2026-06-17/spxml-functions/spxml_object_functions.md"
  - "raw/2026-06-17/spxml-functions/spxml_string_functions.md"
  - "raw/2026-06-17/spxml-functions/spxml_type_conversion_functions (1).md"
  - "raw/2026-06-17/spxml-functions/spxml_url_functions.md"
  - "raw/2026-06-17/spxml-functions/spxml_xml_document_functions.md"
  - "raw/2026-06-17/spxml-functions/spxml_xml_element_functions.md"
status: "draft"
---

# SP-XML: обзор встроенных функций

## Кратко

В `INBOX` добавлен набор markdown-справочников по встроенным функциям SP-XML. Эти материалы относятся к внутреннему серверному языку WebTutor / SP-XML Script и не должны автоматически переноситься на Node.js, браузерные API или внешние ECMAScript-среды.

Полные исходные материалы сохранены в:

```text
raw/2026-06-17/spxml-functions/
```

## Группы функций

### Строки

Источник `spxml_string_functions.md` описывает функции проверки, поиска, фрагментов, замены, разбиения, регистра и работы с символами.

Ключевой нюанс: часть функций работает с байтами, часть - с символами.

- `StrLen()` - длина в байтах.
- `StrCharCount()` - количество символов.
- `StrLeftRange()` - левая часть по длине в байтах.
- `StrLeftCharRange()` - левая часть по длине в символах.
- `StrOptSubStrPos()`, `StrOptSubStrPosB()`, `StrOptSubStrRightPos()` возвращают позиции в байтах.
- `StrCharRangePos()` работает с позициями в символах.

### Массивы

Источник `spxml_array_functions.md` фиксирует, что массив в SP-XML может быть не индексируемым списком, а абстрактным перечислителем или результатом запроса.

Гарантированно прямой доступ поддерживают:

- стандартный массив `Array`;
- объект `XmlElem`.

Если нужен доступ по индексу или многократная обработка результата XQuery, источник рекомендует заранее привести массив через `ArrayDirect()` или `ArraySelectAll()`.

### Даты

Источник `spxml_date_functions.md` описывает функции создания, разбора, проверки, смещения, преобразования зон и форматирования дат:

- `Date()`, `OptDate()`, `ParseDate()`, `ParseMimeDate()`;
- `IsValidDate()`, `DateDiff()`, `DateOffset()`;
- `DateNewTime()`, `DateNewTimeZone()`;
- `DateToLocalDate()`, `DateToTimeZoneDate()`, `UtcToLocalDate()`;
- `StrDate()`, `StrXmlDate()`, `StrMimeDate()` и другие.

В источнике есть спорные места: например, `Date()` может быть недостаточно безопасен для проверки пользовательской даты, а результат `StrMimeDate()` требует сверки.

### URL и файловая система

`spxml_url_functions.md` покрывает работу с URL:

- `AbsoluteUrl()`, `UrlAppendPath()`, `UrlParent()`, `UrlPath()`, `UrlScheme()`;
- `LoadUrlData()`, `LoadUrlText()`, `PutUrlData()`, `PutUrlText()`;
- `UrlExists()`, `UrlFileSize()`, `UrlModDate()`;
- `FilePathToUrl()`, `UrlToFilePath()`, `UrlToOptFilePath()`.

`spxml_filesystem_functions.md` покрывает служебные директории, пути, чтение/запись, копирование, перемещение, удаление, временные файлы и shell-ярлыки.

### XML-документы и XML-элементы

`spxml_xml_document_functions.md` описывает:

- `OpenDoc()`, `OpenNewDoc()`, `OpenDocFromStr()`;
- `FetchDoc()`, `GetCachedDoc()`, `GetOptCachedDoc()`, `GetAllCachedDocs()`;
- `DeleteDoc()`, `MoveDoc()`;
- `ObjectDocUrl()`, `ObjectIDFromUrl()`, `ObjectNameFromUrl()`;
- `RegisterSharedDoc()`, `ReplaceCachedDoc()`, `UpdateUiDoc()`.

Опции открытия XML-документа задаются строкой вида:

```text
param1='value1';param2='value2';...
```

`spxml_xml_element_functions.md` описывает:

- `CreateElemByFormElem()`;
- `CreateElem()`;
- `CreateDynamicElem()`;
- `GetForeignElem()`, `GetOptForeignElem()`, `GetFailedForeignElem()`;
- `LoadElemsFromStr()`, `ExportElemsToStr()`.

### Объекты

`spxml_object_functions.md` дополняет ранее обработанные функции:

- `GetObjectPropertyOrSelf()`;
- `GetObjectPropertyWithLock()`;
- `GetOptObjectProperty()`;
- `SetObjectProperty()`;
- `SetObjectPropertyWithLock()`.

### Выполнение кода

`spxml_code_execution_functions.md` содержит функции выполнения и проверки кода:

- `CheckCodePageSyntax()`, `CheckCodeSyntax()`;
- `CodeLibraryMethodExists()`;
- `OpenCodeLibrary()`, `RegisterCodeLibrary()`, `OpenCodeLib()`;
- `EvalCodePage()`, `EvalCodePageUrl()`, `EvalCodeUrl()`, `EvalCs()`;
- `SafeEval()`, `OptEval()`, `ExtEval()`, `eval()`, `InPlaceEval()`;
- `EvalAsync()`, `EvalSync()` в другом источнике.

TODO: нужно проверить безопасные ограничения для функций `Eval*` перед применением в разработке.

### Форматы, кодировки, HTML, JSON и прочие функции

`spxml_format_encoding_conversion.md` покрывает:

- кодировки;
- MIME;
- Base64 и HEX;
- RTF;
- XML;
- URL и query string;
- multipart/form-data;
- SQL и XQuery литералы;
- встроенное строковое шифрование.

`spxml_html_functions.md` описывает `HtmlEncode()`, `HtmlEncodeDoc()`, `HtmlToPlainText()` и другие функции обработки HTML.

`spxml_misc_functions.md` содержит разнородные функции, включая:

- `EncodeJson()`, `ParseJson()`;
- `HttpRequest()`;
- `PasswordHash()`, `PasswordVerify()`;
- `SHA256()`, `SHA256Hex()`, `SHA1()`, `Md5()`;
- `UniqueID()`, `UniqueStrID()`;
- `Cancel()`, `UserError()`, `UiError()`;
- `ProcessExecute()`, `ShellExecute()`;
- `ZipCreate()`, `ZipExtract()`;
- `alert()`, `DebugMsg()`, `LogEvent()`.

## Важные правила для разработки

- Для пользовательских строк с не-ASCII символами выбирать символьные функции, а не байтовые.
- Результаты XQuery и абстрактные массивы не считать автоматически индексируемыми.
- Функции `Eval*`, `ProcessExecute()`, `ShellExecute()`, `HttpRequest()` требуют отдельной проверки безопасности и контекста выполнения.
- При работе с документами проверять, нужен ли кэш (`FetchDoc()`) или прямое открытие (`OpenDoc()`).

## TODO / вопросы для проверки

- TODO: нужно сверить спорные места в строковых функциях: `StrCharRangePos()`, `StrSplit2()`, `StrReplaceOne()`, описание `StrUpperCase()`.
- TODO: нужно проверить безопасные практики использования `Eval*`, `HttpRequest()`, `ProcessExecute()`, `ShellExecute()`.
- TODO: нужно собрать отдельные рецепты по `OpenDoc()` + `Save()` для типовых объектов WebTutor.
- TODO: нужно проверить, какие функции доступны в конкретной версии WebSoft HCM.

## Источники

- `raw/2026-06-17/spxml-functions/`
