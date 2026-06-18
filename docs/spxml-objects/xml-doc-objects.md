---
title: "XML-объекты SP-XML: XmlDoc, XmlElem, XmlMultiElem"
topic: "spxml-objects/xml"
source_files:
  - "raw/2026-06-18/xml-objects/xml-doc-objects-xmldoc-ai-friendly.md"
  - "raw/2026-06-18/xml-objects/xml-doc-objects-xmlelem-ai-friendly.md"
  - "raw/2026-06-18/xml-objects/xml-doc-objects-xmlmultielem-ai-friendly.md"
status: "draft"
---

# XML-объекты SP-XML: XmlDoc, XmlElem, XmlMultiElem

## Кратко

Новые материалы описывают три связанных объекта внутреннего серверного языка WebTutor / SP-XML:

- `XmlDoc` - XML-документ целиком.
- `XmlElem` - один XML-узел/элемент внутри документа.
- `XmlMultiElem` - виртуальный массив одноименных множественных XML-элементов, описанных в XMD-форме с `MULTIPLE="1"`.

Синтаксис примеров похож на JavaScript, но это серверный код WebTutor / SP-XML, а не Node.js и не браузерный JavaScript.

## XmlDoc

`XmlDoc` представляет XML-документ. Основной доступ к содержимому идет через `TopElem`, который возвращает корневой `XmlElem`.

Ключевые свойства: `TopElem`, `Url`, `DocID`, `ObjectID`, `Form`, `FormUrl`, `HasWriteAccess`, `NeverSaved`, `IsSeparated`, `WriteDocInfo`, `WriteFt`, `WriteAllNodes`, `WriteMaskLineBreaks`, `RunActionOnBeforeSave`.

Ключевые методы: `Save()`, `Save(docUrl)`, `Reload()`, `RemoveFromCache()`, `SetCached()`, `BindToDb([dbName])`, `BindToDbObjectType()`, `SubElem(path)`, `TopOrChildElem(name)`, `SetChanged()`, `UpdateValues()`.

В новом материале указано: для новой объектной модели предпочтительно проверять возможность использования `DefaultDb.OpenNewObjectDoc()` или `XmlDoc.BindToDbObjectType()`. При этом в локальном проверенном кейсе создания `collaborator` рабочим был вариант `tools.new_doc_by_name('collaborator', false)` + `BindToDb(DefaultDb)`.

## XmlElem

`XmlElem` соответствует конкретному XML-узлу в открытом XML-документе. Элемент имеет значение и набор дочерних элементов. К дочерним элементам можно обращаться как к свойствам:

```js
elem.xxx
```

`XmlElem` также обладает массивоподобными свойствами: можно получать список дочерних элементов через индексирование `elem[i]`.

Ключевые свойства: `Name`, `Value`, `Xml`, `XmlValue`, `Parent`, `Doc`, `OptDoc`, `ChildNum`, `ChildIndex`, `IsTopElem`, `IsDynamic`, `IsMultiElem`, `ReadOnly`, `ForeignElem`, `OptForeignElem`.

Ключевые методы работы с дочерними элементами: `AddChild()`, `AddChildElem(elem)`, `AddDynamicChild()`, `Child(name)`, `OptChild(name)`, `ChildExists(name)`, `GetChildByKey(key)`, `GetOptChildByKey(key)`, `ObtainChildByKey(key)`, `ObtainChildByValue(value)`, `EvalPath(path)`, `EvalOptPath(path)`, `EvalMultiPath(path)`.

## Удаление XmlElem

Новый материал подтверждает методы удаления XML-элементов:

- `Delete()` - удаляет текущий элемент.
- `DeleteChildren()` - удаляет дочерние элементы по условию или все.
- `DeleteChildByKey()` - удаляет дочерний элемент по ключу; если не найден, возникает ошибка.
- `DeleteOptChildByKey()` - безопасно удаляет дочерний элемент по ключу; если не найден, ничего не делает.
- `DeleteChildrenByValue()` - удаляет дочерние элементы по значению.
- `Clear()` - очищает значение и дочерние элементы.

Для динамических атрибутов: `DeleteAttr()` удаляет атрибут; если атрибута нет, возникает ошибка. `DeleteOptAttr()` безопасно удаляет атрибут; если атрибута нет, ничего не делает.

Пример из источника:

```js
list.DeleteOptChildByKey( keyValue );
```

Для полной синхронизации с XML-данными перед `LoadData()` / `LoadDataFromUrl()` нужно вызвать `Clear()`, потому что загрузка подгружает только присутствующие в исходном XML данные.

```js
elem.Clear();
elem.LoadData( xmlData );
```

## XmlMultiElem

`XmlMultiElem` - виртуальный объект для работы с множественными XML-элементами, которые в XMD-форме описаны с `MULTIPLE="1"`.

Он не является физическим XML-узлом. Это обертка над набором одноименных дочерних элементов родительского `XmlElem`.

Ключевые методы:

- `Add()` - добавляет новый множественный элемент и возвращает `XmlElem`.
- `ByValueExists(value)` - проверяет наличие элемента с заданным значением.
- `ObtainByValue(value)` - находит элемент по значению или создает новый.
- `SetValues(values)` - заменяет весь набор значений.
- `Clear()` - удаляет все соответствующие множественные элементы.
- `DeleteAll()` - устаревший синоним `Clear()`.
- `DeleteByValue(value)` - удаляет первый элемент с заданным значением; если не найден, возникает ошибка.
- `GetByKey(keyValue[, keyName])` - возвращает элемент с заданным ключом или ошибку.
- `ObtainByKey(keyValue[, keyName])` - находит или создает элемент по ключу.
- `ObtainByKeySorted(keyValue[, keyName])` - находит или вставляет элемент в позицию по возрастанию ключа.

Безопасное удаление значения:

```js
if ( candidate.profession_id.ByValueExists( 2054 ) )
{
    candidate.profession_id.DeleteByValue( 2054 );
}
```

Полная очистка:

```js
candidate.profession_id.Clear();
```

Полная замена:

```js
candidate.profession_id.SetValues( [2054, 2402, 2110] );
```

## Важные нюансы

- Методы без `Opt` часто возвращают ошибку, если элемент/атрибут/путь не найден.
- Для допустимого отсутствия данных предпочтительны `OptChild`, `GetOptChildByKey`, `EvalOptPath`, `OptForeignElem`, `DeleteOptChildByKey`, `DeleteOptAttr`.
- `XmlMultiElem.DeleteByValue(value)` возвращает ошибку, если значения нет; перед удалением безопаснее проверять `ByValueExists(value)`.
- Хранить ссылку на изменяемый `XmlElem` в `Session` опасно: платформа автоматически устанавливает `ReadOnly = true`, чтобы предотвратить небезопасные одновременные изменения из разных потоков.
- Содержимое `XmlDoc` не thread-safe при работе с `EvalThread()`.

## Проверенные факты

- Новый материал подтверждает наличие методов удаления XML-элементов внутри документа: `Delete()`, `DeleteChildren()`, `DeleteChildByKey()`, `DeleteOptChildByKey()`, `DeleteChildrenByValue()`.
- Для `XmlMultiElem` подтверждены `Clear()`, `DeleteByValue(value)`, `DeleteAll()` как устаревший синоним `Clear()`.
- `XmlDoc.Save()` сохраняет по существующему URL или по URL, переданному аргументом.
- `XmlDoc.TopElem` возвращает корневой `XmlElem`.

## TODO / вопросы для проверки

- TODO: нужно проверить подробное поведение методов, которые в источнике перечислены без детального описания: `XmlElem.Child()`, `XmlDoc.RunActionOnSave`, `XmlDoc.WriteAsync`, `XmlDoc.MakeReport()`.
- TODO: нужно проверить, какие из новых методов доступны в конкретной версии WebSoft HCM / WebTutor на стенде.
- TODO: нужно проверить рекомендации по новой объектной модели для создания документов: `DefaultDb.OpenNewObjectDoc()` и `XmlDoc.BindToDbObjectType()`.

## Источники

- `raw/2026-06-18/xml-objects/xml-doc-objects-xmldoc-ai-friendly.md`
- `raw/2026-06-18/xml-objects/xml-doc-objects-xmlelem-ai-friendly.md`
- `raw/2026-06-18/xml-objects/xml-doc-objects-xmlmultielem-ai-friendly.md`
