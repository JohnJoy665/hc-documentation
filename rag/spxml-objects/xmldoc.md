---
title: "XmlDoc"
topic: "spxml-objects/xml-doc"
source_files:
  - "raw/2026-06-18/xml-objects/xml-doc-objects-xmldoc-ai-friendly.md"
status: "draft"
---

# XmlDoc

`XmlDoc` представляет XML-документ целиком. Основной доступ к содержимому идет через `TopElem`, который возвращает корневой `XmlElem`.

Ключевые свойства: `TopElem`, `Url`, `DocID`, `ObjectID`, `Form`, `FormUrl`, `HasWriteAccess`, `NeverSaved`, `IsSeparated`, `WriteDocInfo`, `WriteFt`, `WriteAllNodes`, `WriteMaskLineBreaks`, `RunActionOnBeforeSave`.

Ключевые методы: `Save()`, `Save(docUrl)`, `Reload()`, `RemoveFromCache()`, `SetCached()`, `BindToDb([dbName])`, `BindToDbObjectType()`, `SubElem(path)`, `TopOrChildElem(name)`, `SetChanged()`, `UpdateValues()`.

В новом источнике `BindToDb()` отмечен как устаревший метод; для новой объектной модели нужно проверять `DefaultDb.OpenNewObjectDoc()` или `BindToDbObjectType()`. В локальном проверенном кейсе создания `collaborator` рабочим был `BindToDb(DefaultDb)`.
