---
title: "WebTutor / WebSoft HCM: организация backend API routing через router.html и JS-библиотеки"
source: "Диалог с ChatGPT по сбору AI-friendly документации"
status: "draft"
created_at: "2026-06-17"
language: "ru"
tags:
  - WebTutor
  - WebSoft HCM
  - backend routing
  - server-side javascript
  - router.html
  - OpenCodeLib
  - CallObjectMethod
  - Request
  - Response
  - API
---

# WebTutor / WebSoft HCM: организация backend API routing через `router.html` и JS-библиотеки

## Оглавление

1. [Цель отчёта](#цель-отчёта)
2. [Контекст](#контекст)
3. [Основной паттерн: единый backend router.html](#основной-паттерн-единый-backend-routerhtml)
4. [Исходный пример роутера](#исходный-пример-роутера)
5. [Как работает роутер по шагам](#как-работает-роутер-по-шагам)
6. [Практическая модель: class/action/data](#практическая-модель-classactiondata)
7. [Методы и объекты, разобранные в диалоге](#методы-и-объекты-разобранные-в-диалоге)
   - [AppDirectoryPath](#appdirectorypath)
   - [Server.Execute](#serverexecute)
   - [Request.Query](#requestquery)
   - [Request.Body](#requestbody)
   - [tools.read_object](#toolsread_object)
   - [Object и WT-методы объектов](#object-и-wt-методы-объектов)
   - [DropFormsCache](#dropformscache)
   - [OpenCodeLib](#opencodelib)
   - [CallObjectMethod](#callobjectmethod)
   - [tools.object_to_text](#toolsobject_to_text)
   - [Response.Write](#responsewrite)
   - [x-local](#x-local)
8. [Важные выводы для AI-friendly документации](#важные-выводы-для-ai-friendly-документации)
9. [Риски и ограничения паттерна](#риски-и-ограничения-паттерна)
10. [Что осталось дозаполнить](#что-осталось-дозаполнить)
11. [Черновой текст для будущей документации](#черновой-текст-для-будущей-документации)

---

## Цель отчёта

Цель этого отчёта — зафиксировать найденный и практически используемый способ организации backend API routing в WebTutor / WebSoft HCM для последующей обработки агентом и превращения в более полноценную AI-friendly документацию.

Отчёт не является финальной пользовательской документацией. Это технический исходник: в нём собраны наблюдения, выдержки из документации, практические выводы и важные оговорки, которые нужно учитывать при дальнейшем оформлении.

---

## Контекст

В рамках подготовки локальной AI-friendly документации по WebTutor / WebSoft HCM был разобран пример кастомного backend-роутера, расположенного по пути:

```txt
D:\GMCS\Unirest\Unirest.WT\WebSoftServer\wt\web\ambassador\back\router.html
```

Идея такого роутера — создать единую серверную HTML-страницу, которая:

1. инициализирует пользователя;
2. читает параметры запроса;
3. определяет, какую backend-библиотеку открыть;
4. определяет, какой метод этой библиотеки вызвать;
5. передаёт в метод данные запроса и текущего пользователя;
6. возвращает результат клиенту в JSON.

Такой подход не является единственно возможным для WebTutor, но он удобен для кастомной разработки, потому что рядом с `router.html` можно организовать набор `.js` файлов, которые будут работать как backend-библиотеки / контроллеры / сервисы.

---

## Основной паттерн: единый backend router.html

Паттерн можно назвать так:

```txt
Единый backend router.html для вызова серверных JS-библиотек
```

Краткая суть:

```txt
router.html = единая точка входа
<class>.js = backend-библиотека / controller / service module
action = имя вызываемого метода внутри библиотеки
data = payload запроса
curUser = текущий авторизованный пользователь / auth context
```

Внешний клиент может обращаться к роутеру, передавая `class`, `action` и `data`. Роутер на стороне WebTutor открывает соответствующий `.js` файл и вызывает нужную функцию.

---

## Исходный пример роутера

```js
<%

    Server.Execute(AppDirectoryPath() + "/wt/web/include/user_init.html" );

    var query = Request.Query;
    var body = Request.Body;

    try {

        var bodyObject = tools.read_object(body);

        var classFromQuery = query.GetOptProperty("class", "");
        var actionFromQuery = query.GetOptProperty("action", "");

        var classFromBody = bodyObject.GetOptProperty("class", "");
        var actionFromBody = bodyObject.GetOptProperty("action", "");
        var dataFromBody = bodyObject.GetOptProperty("data", {});

        var class = classFromQuery ? classFromQuery : classFromBody;
        var action = actionFromQuery ? actionFromQuery : actionFromBody;
        var data = dataFromBody;

        var classUrl = 'x-local://wt/web/ambassador/back/' + class + '.js';

        DropFormsCache(classUrl);
        var classLib = OpenCodeLib(classUrl);

        var actionResult = CallObjectMethod(classLib, action, [data, curUser]);

        Response.Write(tools.object_to_text(actionResult, "json"));

    } catch(err) {
    
        errorObj = {
            error: 1,
            message: err + "",
            result: null,
        };

        Response.Write(tools.object_to_text(errorObj, "json"));
    }

%>
```

---

## Как работает роутер по шагам

### 1. Подключение инициализации пользователя

```js
Server.Execute(AppDirectoryPath() + "/wt/web/include/user_init.html" );
```

Этот вызов подключает серверный include-файл `user_init.html`. После его выполнения в текущем серверном окружении становятся доступны переменные, связанные с пользователем, сессией, cookie, host/site context и т.д.

Важный практический результат: далее роутер может передать в backend-метод текущего пользователя:

```js
CallObjectMethod(classLib, action, [data, curUser]);
```

### 2. Получение query-параметров и body

```js
var query = Request.Query;
var body = Request.Body;
```

`Request.Query` используется для чтения параметров URL и POST-формы.

`Request.Body` используется для чтения тела HTTP-запроса строкой.

### 3. Парсинг body

```js
var bodyObject = tools.read_object(body);
```

`tools.read_object` преобразует строку в объект. В данном случае ожидается, что body содержит JSON-строку.

### 4. Чтение class/action/data

```js
var classFromQuery = query.GetOptProperty("class", "");
var actionFromQuery = query.GetOptProperty("action", "");

var classFromBody = bodyObject.GetOptProperty("class", "");
var actionFromBody = bodyObject.GetOptProperty("action", "");
var dataFromBody = bodyObject.GetOptProperty("data", {});
```

Роутер поддерживает два источника параметров:

1. URL/query;
2. JSON body.

Если `class` и `action` переданы в query, они имеют приоритет. Если их нет в query, значения берутся из body.

```js
var class = classFromQuery ? classFromQuery : classFromBody;
var action = actionFromQuery ? actionFromQuery : actionFromBody;
var data = dataFromBody;
```

### 5. Построение пути до backend-библиотеки

```js
var classUrl = 'x-local://wt/web/ambassador/back/' + class + '.js';
```

Например:

```txt
class = "assessment"
```

даст путь:

```txt
x-local://wt/web/ambassador/back/assessment.js
```

### 6. Сброс кэша и открытие библиотеки

```js
DropFormsCache(classUrl);
var classLib = OpenCodeLib(classUrl);
```

`OpenCodeLib` открывает JS-файл как библиотеку функций и возвращает объект, методы которого можно вызвать.

Важно: `DropFormsCache` в современных версиях WebTutor имеет ограничение — с версии `2025.2.1265` функция объявлена устаревшей и не выполняется. Поэтому в новой документации нельзя подавать её как надёжный способ сброса кэша.

### 7. Вызов метода библиотеки

```js
var actionResult = CallObjectMethod(classLib, action, [data, curUser]);
```

`action` — это имя метода внутри открытой библиотеки.

Например, если:

```txt
class = "assessment"
action = "getGoals"
```

то роутер откроет:

```txt
assessment.js
```

и вызовет в нём функцию:

```js
getGoals(data, curUser)
```

### 8. Возврат JSON

```js
Response.Write(tools.object_to_text(actionResult, "json"));
```

Backend-метод возвращает объект, а роутер сериализует его в JSON и пишет в HTTP-ответ.

В случае ошибки используется единый формат:

```js
{
    error: 1,
    message: err + "",
    result: null,
}
```

---

## Практическая модель: class/action/data

Рекомендуемая модель запроса к такому роутеру:

```json
{
  "class": "assessment",
  "action": "getGoals",
  "data": {
    "assessmentId": 123
  }
}
```

Условный backend-файл:

```txt
wt/web/ambassador/back/assessment.js
```

Пример содержимого:

```js
function getGoals(data, curUser) {
    return {
        error: 0,
        message: "",
        result: []
    };
}

function saveGoal(data, curUser) {
    return {
        error: 0,
        message: "",
        result: {
            id: 123
        }
    };
}
```

Рекомендуемый формат успешного ответа:

```js
{
    error: 0,
    message: "",
    result: {}
}
```

Рекомендуемый формат ошибки:

```js
{
    error: 1,
    message: "Текст ошибки",
    result: null
}
```

---

## Методы и объекты, разобранные в диалоге

### AppDirectoryPath

**Статус в документации:** активен.  
**Дата создания:** 06.08.2019.  
**Обновлено:** 10.01.2025.

`AppDirectoryPath()` возвращает путь к директории, из которой запущено приложение, то есть к установочной папке.

Синтаксис:

```js
AppDirectoryPath()
```

Аргументы отсутствуют.

Возвращаемое значение: строка.

Пример:

```js
A1 = AppDirectoryPath();
alert(A1);
```

В контексте роутера используется так:

```js
Server.Execute(AppDirectoryPath() + "/wt/web/include/user_init.html");
```

#### Важное уточнение про тонкий и толстый клиент

В документации указано, что `AppDirectoryPath()` не работает в тонком клиенте, то есть в браузере. Для корректной работы функции рекомендуется переносить обработку данных на сервер.

Исторически у WebTutor была админка в виде Windows-приложения — толстый клиент. Сейчас администрирование и настройки обычно выполняются через тонкий клиент, то есть веб-админку.

Примеры запуска веб-админки локально:

```txt
http://localhost/admin
http://localhost/spxml_web/main.htm
```

Практический вывод: `AppDirectoryPath()` следует рассматривать как серверную функцию / функцию окружения приложения, а не как функцию, доступную в браузере.

---

### Server.Execute

Отдельное описание `Server.Execute` в найденной документации не было обнаружено.

По практическому использованию в WebTutor его можно описать как серверный include / выполнение кода из указанного файла внутри текущего серверного шаблона.

Пример:

```js
Server.Execute(AppDirectoryPath() + "/wt/web/include/user_init.html");
```

Практический смысл:

- подключает код из другого серверного файла;
- позволяет получить в текущем окружении переменные, определённые или подготовленные в подключаемом файле;
- используется для получения данных пользователя, сессии, cookie, host/site context и других переменных окружения.

В контексте router.html `Server.Execute` нужен прежде всего для подключения `include/user_init.html`.

После этого в роутере можно использовать переменную `curUser` и передавать её в backend-методы.

---

### Request.Query

**Статус в документации:** активен.  
**Дата создания:** 29.10.2019.

`Request.Query` — атрибут объекта `Request`.

Назначение: возвращает стандартный JavaScript-объект, содержащий объединённый набор полей из атрибутов `Form` и `QueryString`.

Синтаксис:

```js
Request.Query
```

Возвращаемое значение: JavaScript-объект.

Примеры:

```js
oRequestQuery = Request.Query;
```

```js
sAltMethod = StrLowerCase(Request.Query.GetOptProperty("method", ""));
```

В контексте router.html:

```js
var query = Request.Query;

var classFromQuery = query.GetOptProperty("class", "");
var actionFromQuery = query.GetOptProperty("action", "");
```

Практический вывод: `Request.Query` удобно использовать для чтения параметров, пришедших как через URL, так и через POST-форму.

---

### Request.Body

**Статус в документации:** активен.  
**Дата создания:** 29.10.2019.

`Request.Body` — атрибут объекта `Request`.

Назначение: возвращает тело HTTP-запроса в виде строки.

Синтаксис:

```js
Request.Body
```

Возвращаемое значение: строка.

Для POST-формы может вернуть строку вида:

```txt
name1=test1&name2=test2&submit=Send
```

Примеры:

```js
sBody = Request.Body;
```

```js
reqArg = ParseJson(DecodeCharset(Request.Body, "utf-8"));
```

В контексте router.html:

```js
var body = Request.Body;
var bodyObject = tools.read_object(body);
```

Практический вывод: если frontend отправляет JSON, `Request.Body` получает его как строку, после чего строку нужно преобразовать в объект.

---

### tools.read_object

**Статус в документации:** активен.  
**Дата создания:** 17.04.2019.

`tools.read_object` преобразует строку в объект.

Поддерживаемые варианты:

- строка в формате JSON;
- строка, содержащая XML.

Синтаксис по смыслу:

```js
tools.read_object(sSomeObject)
```

Входной параметр:

```txt
sSomeObjectPARAM — строка в формате JSON или XML
```

Возвращаемый результат: объект.

Примеры:

```js
oResult = tools.read_object(sResult);
```

```js
oUrlResult = tools.read_object(call_method("getSaveFileUrl", oParam, "json"));
```

В контексте router.html:

```js
var bodyObject = tools.read_object(body);
```

Практический вывод: метод удобен как универсальный парсер body в router.html.

---

### Object и WT-методы объектов

**Object (JavaScript Object)**  
**Статус:** активен.  
**Дата создания:** 26.10.2019.  
**Обновлено:** 10.03.2021.

В WT-среде для JavaScript-объекта по умолчанию определены дополнительные методы/атрибуты:

- `HasProperty`
- `AddProperty`
- `SetProperty`
- `GetProperty`
- `GetOptProperty`

Пример создания объекта:

```js
var obj = new Object();
```

#### HasProperty

**Статус:** активен.  
**Дата создания:** 10.03.2021.  
**Обновлено:** 09.12.2025.

Проверяет, существует ли атрибут с заданным именем.

Синтаксис:

```js
obj.HasProperty(propName)
```

Результат: boolean.

Пример:

```js
if (!Session.HasProperty("sid")) {
    Session.sid = Random(0, 1152921504606846976);
}
```

#### AddProperty

**Статус:** активен.  
**Дата создания:** 10.03.2021.  
**Обновлено:** 09.12.2022.

Добавляет в объект новый атрибут и присваивает ему значение.

Синтаксис:

```js
obj.AddProperty(propName, propVal)
```

Аргументы:

- `propName` — строка, имя атрибута;
- `propVal` — значение атрибута.

Возвращаемое значение: `undefined`.

Пример:

```js
obj.AddProperty("param1", 1);
```

#### SetProperty

**Статус:** активен.  
**Дата создания:** 10.03.2021.  
**Обновлено:** 09.12.2022.

Устанавливает или изменяет значение атрибута объекта. Если атрибут отсутствует — добавляет его.

Синтаксис:

```js
obj.SetProperty(propName, propVal)
```

Пример:

```js
obj.SetProperty("param1", 1);
```

Практический пример:

```js
oParam.SetProperty("insert_params", sInsertParams);
```

#### GetOptProperty

**Статус:** активен.  
**Дата создания:** 10.03.2021.  
**Обновлено:** 23.03.2023.

Возвращает значение атрибута объекта. Если атрибут отсутствует, возвращает значение по умолчанию из второго аргумента. Если второй аргумент не указан — возвращает `undefined`.

Синтаксис:

```js
obj.GetOptProperty(propName, defaultVal)
```

Пример:

```js
AppConfig.GetOptProperty("SVG-CACHE-TIMEOUT", 3600);
```

В router.html:

```js
var classFromQuery = query.GetOptProperty("class", "");
var actionFromQuery = query.GetOptProperty("action", "");
var dataFromBody = bodyObject.GetOptProperty("data", {});
```

Практический вывод: `GetOptProperty` — основной безопасный способ читать необязательные поля, особенно входные параметры от клиента.

#### GetProperty

**Статус:** активен.  
**Дата создания:** 10.03.2021.  
**Обновлено:** 09.12.2022.

Возвращает значение атрибута объекта. Если атрибут отсутствует — выбрасывает ошибку.

Синтаксис:

```js
obj.GetProperty(propName)
```

Пример:

```js
AppConfig.GetProperty("alt-app-name");
```

Важное отличие от `GetOptProperty`:

```js
anyObj4 = obj.GetProperty("param3");
```

Если `param3` отсутствует, будет ошибка:

```txt
Элемент не найден
```

Практический вывод: `GetProperty` лучше использовать только для обязательных внутренних настроек, отсутствие которых действительно должно останавливать выполнение.

---

### DropFormsCache

**Статус в документации:** активен, но фактически устаревший.  
**Дата создания:** 15.08.2019.  
**Обновлено:** 29.09.2025.

Назначение: удаляет определённые формы из кэша.

Синтаксис:

```js
DropFormsCache(urlPattern)
```

Аргумент:

```txt
urlPattern — обязательная строка, URL формы/XMD-файла или маска URL формы
```

Возвращаемое значение отсутствует.

Примеры:

```js
DropFormsCache("*candidate*");
```

Удаляет из кэша все формы, в названии которых есть `candidate`.

```js
for (objectType in object_types.items) {
    DropFormsCache(objectType.object_form_url);
}
```

Удаляет из кэша формы типов, указанных в `object_types.items`.

В старом router.html:

```js
DropFormsCache(classUrl);
var classLib = OpenCodeLib(classUrl);
```

#### Важное ограничение

С версии `2025.2.1265` функция объявлена устаревшей и не выполняется в коде.

Практический вывод для документации: в новых материалах `DropFormsCache` нужно пометить как deprecated. Нельзя объяснять её как гарантированно рабочий способ сброса кэша JS-библиотеки.

---

### OpenCodeLib

**Статус в документации:** активен.  
**Дата создания:** 30.09.2019.  
**Обновлено:** 21.01.2021.

`OpenCodeLib` открывает документ-библиотеку.

Синтаксис:

```js
OpenCodeLib(url)
```

Аргумент:

```txt
url — обязательная строка, URL загружаемого документа с расширением XML или JS
```

Возвращаемое значение: объект `XmlDoc` / ссылка на документ.

Назначение:

- если открыт XML-документ, содержащий набор методов, возвращает его корневой элемент;
- если открыт JS-файл с функциями, возвращает псевдо-документ / псевдо-форму, содержащую эти функции;
- после этого методы и функции можно выполнять.

Примеры:

```js
lib = OpenCodeLib("x-app://gate/gate_lib_gate.js");
config = lib.get_browser_plugin_config();
```

```js
var test_lib = OpenCodeLib("x-local://custom_folder//new_lib.js");
```

```js
OpenCodeLib("lib_backup.js").RunBackup();
```

В router.html:

```js
var classLib = OpenCodeLib(classUrl);
```

#### Отличие от EvalCodeUrl

По документации действие похоже на `EvalCodeUrl`, но не тождественно ему.

При выполнении сторонних функций через `EvalCodeUrl` переменные внешнего кода видны внутри сторонних функций. Это может приводить к ошибкам при совпадении имён переменных.

`OpenCodeLib` лишён этого недостатка, потому что загружает функции из внешнего источника как независимые методы. Каждый метод выполняется в собственном окружении.

Ограничение: загружаемый JS-файл не может содержать глобальных переменных.

Практический вывод: для backend-библиотек, вызываемых из router.html, `OpenCodeLib` выглядит предпочтительнее, чем `EvalCodeUrl`, так как снижает риск конфликтов переменных.

---

### CallObjectMethod

**Статус в документации:** активен.  
**Дата создания:** 10.11.2020.  
**Обновлено:** 10.11.2020.

`CallObjectMethod` — функция ядра, которая удалённо, на сервере, вызывает метод с параметрами для конкретного объекта.

Синтаксис:

```js
CallObjectMethod(object, method, params)
```

Аргументы:

- `object` — обязательный объект, для которого производится вызов метода;
- `method` — обязательная строка, название вызываемого метода;
- `params` — необязательный массив параметров.

Порядок элементов в массиве `params` должен соответствовать порядку параметров вызываемого метода.

Если параметры не нужны, передаётся пустой массив:

```js
[]
```

Возвращаемое значение зависит от вызываемого метода: строка, флаг, объект и т.д.

Пример:

```js
oLib = OpenCodeLib("x-local://libs/some_library.js");
oResult = CallObjectMethod(oLib, "some_function", [param1, param2]);
```

Важное отличие:

```js
oResult = oLib.some_function(param1, param2);
```

выполняется там, где вызвано — на клиенте или сервере.

А:

```js
oResult = CallObjectMethod(oLib, "some_function", [param1, param2]);
```

всегда выполняется на сервере, а результат возвращается в место вызова.

В router.html:

```js
var actionResult = CallObjectMethod(classLib, action, [data, curUser]);
```

Практический вывод: `CallObjectMethod` хорошо подходит для динамического вызова метода, имя которого приходит строкой из запроса.

---

### tools.object_to_text

**Статус в документации:** активен.  
**Дата создания:** 17.04.2019.  
**Обновлено:** 21.02.2023.

`tools.object_to_text` преобразует объект в строку указанного формата.

Поддерживаемые форматы:

- `json`
- `xml`

Синтаксис по смыслу:

```js
tools.object_to_text(object, format, maxDepth)
```

Параметры:

- `objectPARAM` — объект для преобразования;
- `sFormatPARAM` — строка формата, `json` или `xml`;
- `iMaxDepthPARAM` — необязательный int, по умолчанию `0`, максимальная глубина дочерних свойств объекта, не больше 5.

Ранее существовал четвёртый параметр `sName` для названия XML-тега, но сейчас он не применяется.

Возвращаемое значение: строка.

Примеры:

```js
docElem.TopElem.result = tools.object_to_text(RESULT, "json");
```

```js
alert(tools.object_to_text(doc.TopElem, "xml"));
alert(tools.object_to_text(doc.TopElem, "json"));
```

В router.html:

```js
Response.Write(tools.object_to_text(actionResult, "json"));
```

Практический вывод: backend-функции могут возвращать обычный WT/JS-объект, а роутер централизованно превращает его в JSON-строку.

---

### Response.Write

**Статус в документации:** активен.  
**Дата создания:** 04.11.2019.

`Response.Write` — метод объекта `Response`.

Назначение: записывает содержимое строки в тело HTTP-ответа.

Содержимое предварительно накапливается в буфере или сразу направляется в сеть в зависимости от значения атрибута `WriteMode`.

Именно с помощью этого метода транслируется конструкция вида:

```html
<%=xxx%>
```

в ASP-страницах.

Синтаксис:

```js
Response.Write(str, encode)
```

Аргументы:

- `str` — обязательная строка, которая выводится в тело HTTP-ответа;
- `encode` — необязательный boolean, параметр HTML-маскирования данных.

Значения `encode`:

- `true` — производится маскирование данных по правилам HTML;
- `false` — маскирование не производится.

По умолчанию `encode = false`.

Возвращаемое значение отсутствует.

Примеры:

```js
Response.Write(Response.OrigRequest.Url);
```

```js
Response.Write(answer.name);
```

```js
Response.Write("Оригинальный URL: " + sUrl);
```

```js
Response.Write(EncodeCharset(_sResponseText, "utf-8"));
```

В router.html:

```js
Response.Write(tools.object_to_text(actionResult, "json"));
```

В блоке ошибки:

```js
Response.Write(tools.object_to_text(errorObj, "json"));
```

Практический вывод: `Response.Write` является финальной точкой отправки результата клиенту.

---

### x-local

**Статус в документации:** активен.  
**Дата создания:** 27.04.2023.  
**Обновлено:** 27.04.2023.

`x-local` — это схема обращения к файлу в папке клиента `WebTutorAdmin`.

Пример:

```txt
x-local://custom_library/custom_file.js
```

По документации это обращение к файлу:

```txt
WebTutorAdmin/custom_library/custom_file.js
```

#### Важное уточнение про серверную папку

Чтобы в этом примере `x-local` обращался к папке сервера `WebSoftServer`, а не к папке клиента `WebTutorAdmin`, нужно явно определить подпапку `custom_library` в файле:

```txt
WebTutorAdmin/SpXml.ini
```

Настройка:

```ini
SHARED-DATABASE: custom_library
```

В router.html используется путь:

```js
var classUrl = 'x-local://wt/web/ambassador/back/' + class + '.js';
```

Практический вывод: `x-local://` не следует автоматически трактовать как путь к серверной папке. Фактическое разрешение пути зависит от конфигурации конкретной установки, в частности от `SpXml.ini` и `SHARED-DATABASE`.

Для будущей документации важно явно отметить этот момент, чтобы агент не сделал неверный вывод, что `x-local://wt/web/...` всегда означает физический путь внутри `WebSoftServer`.

---

## Важные выводы для AI-friendly документации

### 1. Router.html можно описывать как backend endpoint

Хотя файл имеет расширение `.html`, по сути он может работать как серверный backend endpoint.

Он принимает запрос, выполняет серверный JS-код и возвращает JSON.

### 2. JS-файлы рядом с роутером можно описывать как backend-библиотеки

Файлы вида:

```txt
assessment.js
users.js
reports.js
```

могут использоваться как набор серверных методов. Их можно открывать через `OpenCodeLib` и вызывать методы через `CallObjectMethod`.

### 3. class/action/data — удобная абстракция для кастомного API

Параметры:

```txt
class  — имя backend-библиотеки
action — имя метода внутри библиотеки
data   — данные запроса
```

удобны для построения простого роутинга без создания отдельного файла под каждый endpoint.

### 4. curUser должен передаваться явно

После подключения `include/user_init.html` в окружении появляется текущий пользователь. Его удобно передавать в каждый backend-метод:

```js
CallObjectMethod(classLib, action, [data, curUser]);
```

Это делает методы контекстными и позволяет внутри них проверять права, ID пользователя, состояние сессии и т.д.

### 5. GetOptProperty — основной безопасный способ читать входные параметры

Для входных данных от клиента лучше использовать:

```js
GetOptProperty("field", defaultValue)
```

а не `GetProperty`, потому что `GetProperty` падает при отсутствии свойства.

### 6. OpenCodeLib предпочтительнее EvalCodeUrl для библиотек

`OpenCodeLib` загружает функции как независимые методы и снижает риск конфликтов переменных.

Но JS-файлы, открываемые через `OpenCodeLib`, не должны содержать глобальных переменных.

### 7. DropFormsCache нельзя считать актуальным решением для новых версий

С версии `2025.2.1265` `DropFormsCache` устарела и не выполняется. В новых материалах нужно либо убрать её из рекомендуемого кода, либо явно пометить как исторический элемент старых примеров.

---

## Риски и ограничения паттерна

### 1. Небезопасный динамический class

В исходном примере `class` приходит от клиента и напрямую используется для построения пути:

```js
var classUrl = 'x-local://wt/web/ambassador/back/' + class + '.js';
```

Это удобно, но потенциально опасно.

Рекомендуется использовать whitelist разрешённых классов:

```js
var allowedClasses = {
    "assessment": true,
    "users": true,
    "reports": true
};

if (!allowedClasses.GetOptProperty(class, false)) {
    throw "Unknown API class: " + class;
}
```

Или аналогичную проверку доступным способом в WT-среде.

### 2. Небезопасный динамический action

`action` также приходит от клиента:

```js
CallObjectMethod(classLib, action, [data, curUser]);
```

Рекомендуется ограничивать список разрешённых методов.

Пример:

```js
var allowedActions = {
    "getGoals": true,
    "saveGoal": true,
    "deleteGoal": true
};

if (!allowedActions.GetOptProperty(action, false)) {
    throw "Unknown API action: " + action;
}
```

### 3. Неочевидное поведение x-local

`x-local://` по документации указывает на папку клиента `WebTutorAdmin`, а не автоматически на сервер `WebSoftServer`.

Для обращения к серверной папке может требоваться настройка `SHARED-DATABASE` в `SpXml.ini`.

### 4. Устаревший DropFormsCache

Старые примеры могут использовать:

```js
DropFormsCache(classUrl);
```

Но с версии `2025.2.1265` функция не выполняется. Агенту нужно учитывать это при генерации новых рекомендаций.

### 5. Нет явной установки Content-Type

В разобранном примере JSON возвращается через:

```js
Response.Write(tools.object_to_text(actionResult, "json"));
```

В чате не разбирался способ установки HTTP-заголовка `Content-Type: application/json`. Этот момент нужно проверить отдельно в документации WebTutor.

---

## Что осталось дозаполнить

Следующие темы были обозначены как полезные, но в этом диалоге ещё не разобраны полностью:

1. `tools_web.user_init` / `include/user_init.html`
   - пользователь отметил, что эта информация уже была передана раньше;
   - для итоговой документации нужно объединить этот отчёт с ранее собранными данными по авторизации и `user_init`.

2. `EvalCodeUrl`
   - упоминается в описании `OpenCodeLib`;
   - нужно отдельно найти описание и зафиксировать отличие.

3. `tools.call_code_library_method`
   - упоминается в описании `CallObjectMethod`;
   - нужно понять, когда использовать его, а когда `OpenCodeLib + CallObjectMethod`.

4. Работа с HTTP-заголовками ответа
   - как в WebTutor корректно установить `Content-Type: application/json`;
   - есть ли методы `Response.SetHeader`, `Response.ContentType` или аналоги.

5. Дополнительные методы, потенциально полезные для backend API:
   - `EncodeJson`
   - `ParseJson`
   - `DecodeCharset`
   - `EncodeCharset`
   - `Response.WriteMode`
   - `Request.Form`
   - `Request.QueryString`
   - `Request.AuthUserID`
   - `Request.Session` / `Session`
   - `Request.DropSession`
   - `tools_web.set_cookie_auth`

---

## Черновой текст для будущей документации

Ниже черновой фрагмент, который агент может использовать как основу для friendly-документации.

### Организация кастомного backend API через router.html

В WebTutor / WebSoft HCM можно организовать кастомный backend API через серверный HTML-файл, например `router.html`. Такой файл работает не как обычная статическая HTML-страница, а как серверный обработчик: внутри него выполняется server-side JavaScript в ASP-подобных блоках `<% ... %>`.

Один из удобных подходов — сделать единый `router.html`, который принимает параметры `class`, `action` и `data`, открывает нужную серверную JS-библиотеку и вызывает в ней нужную функцию.

Пример общей схемы:

```txt
router.html
  ├── читает Request.Query и Request.Body
  ├── определяет class/action/data
  ├── открывает x-local://.../<class>.js через OpenCodeLib
  ├── вызывает action через CallObjectMethod
  └── возвращает JSON через Response.Write
```

Пример запроса:

```json
{
  "class": "assessment",
  "action": "getGoals",
  "data": {
    "assessmentId": 123
  }
}
```

Пример backend-библиотеки:

```js
function getGoals(data, curUser) {
    return {
        error: 0,
        message: "",
        result: []
    };
}
```

Такой подход позволяет хранить backend-логику рядом с роутером в виде набора JS-файлов:

```txt
wt/web/ambassador/back/router.html
wt/web/ambassador/back/assessment.js
wt/web/ambassador/back/users.js
wt/web/ambassador/back/reports.js
```

При этом нужно учитывать несколько важных ограничений:

1. `class` и `action`, приходящие от клиента, желательно проверять через whitelist.
2. `x-local://` по документации указывает на папку клиента `WebTutorAdmin`, а обращение к серверной папке может зависеть от настройки `SHARED-DATABASE` в `SpXml.ini`.
3. `DropFormsCache` в новых версиях WebTutor устарел и не выполняется, поэтому не должен использоваться как обязательный механизм обновления кэша.
4. `OpenCodeLib` открывает JS-файл как библиотеку независимых методов; такой JS-файл не должен содержать глобальных переменных.
5. Результат backend-метода удобно возвращать как объект и централизованно сериализовать через `tools.object_to_text(result, "json")`.

---

## Краткая карта методов

| Метод / объект | Назначение в router.html |
|---|---|
| `AppDirectoryPath()` | Получить путь к установочной/рабочей директории приложения |
| `Server.Execute(...)` | Подключить серверный include, например `user_init.html` |
| `Request.Query` | Получить query/form параметры запроса |
| `Request.Body` | Получить тело HTTP-запроса строкой |
| `tools.read_object(...)` | Преобразовать JSON/XML строку в объект |
| `GetOptProperty(...)` | Безопасно прочитать свойство объекта с дефолтным значением |
| `x-local://...` | Сослаться на файл через схему x-local, с учётом особенностей WebTutorAdmin/WebSoftServer |
| `DropFormsCache(...)` | Исторически — сбросить кэш формы; в новых версиях deprecated |
| `OpenCodeLib(...)` | Открыть JS/XML библиотеку как объект с методами |
| `CallObjectMethod(...)` | Вызвать метод объекта на сервере по имени |
| `tools.object_to_text(..., "json")` | Сериализовать объект в JSON-строку |
| `Response.Write(...)` | Записать строку в HTTP-ответ |

---

## Итог

В этом диалоге был зафиксирован практический способ организации backend API routing в WebTutor / WebSoft HCM через единый `router.html` и набор серверных JS-библиотек. Были разобраны ключевые функции и объекты, на которых держится этот паттерн: `Request`, `Response`, `OpenCodeLib`, `CallObjectMethod`, `tools.read_object`, `tools.object_to_text`, методы WT-объектов и схема `x-local://`.

Главный вывод: такой роутер можно использовать как понятную и компактную архитектурную заготовку для кастомных API в WebTutor, но при подготовке production-friendly документации нужно обязательно добавить проверки `class/action`, уточнить поведение `x-local://`, убрать или пометить как deprecated `DropFormsCache`, а также отдельно дозаполнить тему заголовков ответа и авторизации через `user_init`.
