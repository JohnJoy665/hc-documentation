---
title: "Logout не разлогинивает пользователя из-за Basic-авторизации узла"
system: "WebSoft HCM / WebTutor"
version: "2023.2.967"
case_type: "auth"
status: "verified"
date: "2026-06-16"
tags:
  - logout
  - authentication
  - basic
  - cookie
  - form-based
  - node
  - portal_auth_type
  - tools_web.user_init
  - x-auth-id
  - loggedoff.html
---

# Кейс: logout не разлогинивает пользователя из-за Basic-авторизации узла

## Оглавление

1. [Контекст кейса](#контекст-кейса)
2. [Исходная проблема](#исходная-проблема)
3. [Окружение](#окружение)
4. [Фактическое поведение до исправления](#фактическое-поведение-до-исправления)
5. [Первичная гипотеза: проблема в logout](#первичная-гипотеза-проблема-в-logout)
6. [Проверка `/logout`](#проверка-logout)
7. [Почему кастомный logout-файл не выполнялся](#почему-кастомный-logout-файл-не-выполнялся)
8. [Проверка `tools_web.user_init` после logout](#проверка-tools_webuser_init-после-logout)
9. [Проверенная, но неподтвержденная гипотеза про `xhttp_config.json`](#проверенная-но-неподтвержденная-гипотеза-про-xhttp_configjson)
10. [Файл `loggedoff.html` как ключ к проблеме](#файл-loggedoffhtml-как-ключ-к-проблеме)
11. [Корневая причина](#корневая-причина)
12. [Решение](#решение)
13. [Результат после исправления](#результат-после-исправления)
14. [Важный вывод про `oUserInit.access`](#важный-вывод-про-ouserinitaccess)
15. [Что важно для кастомных API](#что-важно-для-кастомных-api)
16. [Что не помогло](#что-не-помогло)
17. [Диагностический чек-лист](#диагностический-чек-лист)
18. [Ключевые слова для поиска в документации и комьюнити](#ключевые-слова-для-поиска-в-документации-и-комьюнити)
19. [Финальный краткий вывод](#финальный-краткий-вывод)

---

## Контекст кейса

Исследовалась авторизация и выход пользователя на локальном учебном стенде WebSoft HCM / WebTutor.

Цель была практическая: после нажатия кнопки «Выход» пользователь должен был оказаться в состоянии, где можно снова ввести логин и пароль, в том числе логин и пароль другого пользователя.

На практике после logout пользователь не попадал в полноценное анонимное состояние. При повторном переходе на портал вход происходил без повторного ввода логина и пароля.

Кейс важен для будущей AI-friendly документации, потому что он показывает связь между:

- настройкой узла;
- типом авторизации портала;
- `tools_web.user_init`;
- `x-auth-id`;
- Basic Auth;
- `/logout`;
- `/loggedoff.html`;
- поведением браузера после logout.

---

## Исходная проблема

После клика на кнопку «Выход» выполнялся запрос:

```http
GET /logout HTTP/1.1
```

Сервер возвращал:

```http
HTTP/1.1 302 Found
Location: /loggedoff.html
```

Затем открывалась страница:

```text
/loggedoff.html
```

На странице было сообщение:

```text
Вы успешно вышли из системы.
Теперь Вы можете закрыть страницу.
```

Если после этого перейти на:

```text
http://localhost/
```

то загружался портал без формы ввода логина и пароля. Отображалась только кнопка «Вход». После нажатия на кнопку пользователь снова попадал на портал под старым пользователем.

Ожидаемое поведение:

```text
/logout
→ очистка авторизации
→ переход к форме логина/пароля
→ возможность ввести данные другого пользователя
```

Фактическое поведение до исправления:

```text
/logout
→ /loggedoff.html
→ повторный переход на портал
→ вход под старым пользователем без ввода логина/пароля
```

---

## Окружение

Версия системы:

```text
WebSoft HCM 2023.2.967
Дата сборки: 2024-08-09
```

Локальный адрес портала:

```text
http://localhost/
```

Локальный сервер работал через xHTTP / Kestrel.

В `xhttp_config.json` была секция:

```json
"Authentication":
[
  {
    "Host": "*",
    "Scheme": "None",
    "Exclusions": [
      "/default",
      "/lrs/*",
      "/webtutor/*",
      "/verify.html",
      "/token_redirect_for_omnicode.html",
      "/mobileservice/datafeed.xml*"
    ],
    "TicketStoreExpiration": "01:00:00",
    "BearerTokenAllowed": true,
    "LogoutPath": "/logout",
    "RedirectLoggedOffUri": "/loggedoff.html"
  }
]
```

Эта секция важна, потому что именно она определяет встроенный путь выхода:

```text
/logout
```

и страницу после выхода:

```text
/loggedoff.html
```

---

## Фактическое поведение до исправления

После входа на портал браузер отправлял на сервер не только cookie, но и Basic Authorization:

```http
Authorization: Basic dXNlcjE6dXNlcjE=
Cookie: SessionID=2197528065264855286; x-auth-id=a7ee26de24f44d1fa8bc0a2a6fb51e20
```

Строка:

```text
dXNlcjE6dXNlcjE=
```

соответствует:

```text
user1:user1
```

То есть браузер продолжал отправлять старые Basic credentials.

После logout встроенный обработчик действительно удалял cookie:

```http
Set-Cookie: x-auth-id=rm; expires=Sat, 01 Jan 2000 00:00:00 GMT; path=/
Set-Cookie: user_login=rm; expires=Sat, 01 Jan 2000 00:00:00 GMT; path=/
Set-Cookie: user_password=rm; expires=Sat, 01 Jan 2000 00:00:00 GMT; path=/
```

Но Basic Auth не является cookie. Его нельзя очистить таким же способом через `Set-Cookie`.

Поэтому следующий запрос снова приходил с:

```http
Authorization: Basic ...
```

И WebTutor снова определял пользователя как авторизованного.

---

## Первичная гипотеза: проблема в logout

Сначала предполагалось, что проблема в коде logout.

Был найден logout-файл с таким кодом:

```html
<%
Response.WriteMode = 'single';
Response.AddHeader( "Cache-Control", "no-cache, no-store, must-revalidate" );

logoutData = new SafeObject;
logoutData.prevLogin = Request.AuthLogin;
logoutData.prevRejectCount = 0;

try
{
	Server.Execute( AppDirectoryPath() +  "/wt/web/include/host_init.html" );

	switch ( curHost.portal_auth_type )
	{
		case "cookie":
		{
			tools_web.set_cookie_auth( Request, curHost, null );
			break;
		}
	}
}
catch( _no_wt_ ) {}

Request.DropSession();
Request.Session.logoutData = logoutData;

reqParam = UrlQuery( Request.Url );
Request.Redirect( 'logout2.htm?' + UrlEncodeQuery( reqParam ) );
%>
```

Этот файл:

- отключает кэширование;
- пытается выполнить `host_init.html`;
- если тип авторизации хоста равен `cookie`, вызывает `tools_web.set_cookie_auth(Request, curHost, null)`;
- вызывает `Request.DropSession()`;
- редиректит на `logout2.htm`.

На этом этапе было неясно, выполняется ли именно этот файл при переходе на `/logout`.

---

## Проверка `/logout`

Для проверки в logout-файл был добавлен диагностический заголовок:

```js
Response.AddHeader("X-Debug-Logout-Handler", "custom-logout-file-v1");
```

Ожидание:

если выполняется именно этот файл, в response headers запроса `/logout` должен появиться заголовок:

```http
X-Debug-Logout-Handler: custom-logout-file-v1
```

Фактический ответ `/logout` был таким:

```http
HTTP/1.1 302 Found
Content-Length: 0
Date: Tue, 16 Jun 2026 07:47:43 GMT
Location: /loggedoff.html
Set-Cookie: x-auth-id=rm; expires=Sat, 01 Jan 2000 00:00:00 GMT; path=/
Set-Cookie: user_login=rm; expires=Sat, 01 Jan 2000 00:00:00 GMT; path=/
Set-Cookie: user_password=rm; expires=Sat, 01 Jan 2000 00:00:00 GMT; path=/
```

Диагностического заголовка не было.

Также `Location` указывал на:

```text
/loggedoff.html
```

А кастомный logout-файл должен был редиректить на:

```text
logout2.htm
```

---

## Почему кастомный logout-файл не выполнялся

Проверка показала, что при запросе:

```text
/logout
```

выполняется не найденный кастомный logout-файл, а встроенный обработчик xHTTP из `xhttp_config.json`.

На это указывали признаки:

```text
Location: /loggedoff.html
```

и отсутствие:

```text
X-Debug-Logout-Handler: custom-logout-file-v1
```

Причина — в конфигурации:

```json
"LogoutPath": "/logout",
"RedirectLoggedOffUri": "/loggedoff.html"
```

То есть путь `/logout` перехватывался встроенным механизмом выхода xHTTP.

Важно: встроенный logout не был «сломанный». Он действительно очищал cookie:

```text
x-auth-id
user_login
user_password
```

Но он не мог очистить Basic Authorization, сохраненный браузером.

---

## Проверка `tools_web.user_init` после logout

Для диагностики использовался тестовый HTML-хендлер, который вызывал:

```js
oUserInit = tools_web.user_init(Request, Request.Query);
Env = Request.Session.GetOptProperty("Env", ({}));
```

После logout до исправления результат был:

```text
oUserInit.access = true
oUserInit.error_code =
oUserInit.auth_type = basic
oUserInit.auth_login = user1
Env.curUserID = 6148914691236517121
Env.curUser =
Request.AuthUserID = 6148914691236517121
```

Это был главный диагностический результат.

Он означал:

```text
После logout пользователь снова авторизуется через Basic Auth.
```

То есть проблема не в том, что `x-auth-id` не удалился. Он удалялся.

Проблема была в том, что браузер продолжал отправлять:

```http
Authorization: Basic ...
```

А `tools_web.user_init` принимал эту авторизацию и снова определял пользователя `user1`.

---

## Проверенная, но неподтвержденная гипотеза про `xhttp_config.json`

Была проверена гипотеза, что проблема может быть связана с тем, что `/default` находится в `Exclusions` секции `Authentication`.

Исходная настройка:

```json
"Exclusions": [
  "/default",
  "/lrs/*",
  "/webtutor/*",
  "/verify.html",
  "/token_redirect_for_omnicode.html",
  "/mobileservice/datafeed.xml*"
]
```

Гипотеза:

```text
Так как переход на / фактически открывает /default, исключение /default из Scheme: None могло приводить к fallback в Basic Auth.
```

Был выполнен эксперимент: `/default` временно убрали из `Exclusions`.

Результат:

```text
Проблема не исчезла.
Повторный вход без ввода логина и пароля продолжал срабатывать.
```

Вывод:

```text
Гипотеза про /default в Exclusions проверена и не подтвердилась.
```

---

## Файл `loggedoff.html` как ключ к проблеме

Затем был изучен файл `loggedoff.html`.

В начале он выполняет:

```html
<%
Server.Execute( AppDirectoryPath() +  "/wt/web/include/host_init.html" );

switch ( curHost.portal_auth_type )
{
	case "ldap":
	case "cookie":
	{
		tools_web.set_cookie_auth( Request, curHost, null );
		break;
	}
}
Request.DropSession();
%>
```

Далее в файле есть ключевое условие:

```js
if ( curHost.portal_auth_type != "basic" && curHost.portal_auth_type != "ntlm" )
{
%>
<!DOCTYPE html>
<html lang="ru">
<body onload="window.location = '/'"></body>
</html>
<%
}
else
{
%>
...
<div class="wt-text">Вы успешно вышли из системы.</div>
<div class="wt-text-small">Теперь Вы можете закрыть страницу.</div>
...
<%
}
%>
```

До исправления пользователь видел именно страницу:

```text
Вы успешно вышли из системы.
Теперь Вы можете закрыть страницу.
```

Это означало, что выполнялась ветка `else`.

А ветка `else` выполняется только если:

```text
curHost.portal_auth_type == "basic"
```

или:

```text
curHost.portal_auth_type == "ntlm"
```

Так файл `loggedoff.html` указал на корневую проблему: текущий узел портала работал как Basic/NTLM-узел.

---

## Корневая причина

Корневая причина была в настройке узла.

Узел `localhost` был настроен на Basic-авторизацию.

Из-за этого:

1. При входе на портал браузер получал/сохранял Basic credentials.
2. После logout xHTTP удалял `x-auth-id`, `user_login`, `user_password`.
3. Но браузер продолжал отправлять `Authorization: Basic`.
4. `tools_web.user_init` снова авторизовал пользователя через Basic.
5. В результате повторный вход происходил без ввода логина и пароля.

Итоговая причина:

```text
Неверный для локального тестирования тип авторизации узла.
```

Для сценария, где нужно после logout вводить логин и пароль другого пользователя, Basic Auth неудобен, потому что браузер кэширует Basic credentials.

---

## Решение

В интерфейсе администратора был открыт раздел:

```text
Системное администрирование → Узлы
```

В карточке узла для `localhost` был изменен тип авторизации.

Было:

```text
Basic
```

или режим, приводящий к `portal_auth_type = basic`.

Стало:

```text
cookie / Form-Based
```

После изменения дополнительных переменных заполнять не потребовалось.

На карточке узла были видны переменные:

```text
auth_login_eval
auth_pass_eval
auth_prev_eval
login_case_sensitive
login_domen_sensitive
auth_cookie_login
auth_cookie_pass
auth_cookie_domain
auth_cookie_period
disp_login_pass
redirect_login
redirect_url_eval
```

Но для текущего локального стенда оказалось достаточно сменить сам тип авторизации на:

```text
cookie / Form-Based
```

После этого форма входа стала отображаться корректно.

---

## Результат после исправления

После изменения типа авторизации узла на:

```text
cookie / Form-Based
```

при выходе пользователь стал попадать на нормальную форму входа.

Форма содержала:

```text
Логин
Пароль
Вход
Сбросить пароль
Регистрация
```

Это означало, что после logout теперь можно заново ввести логин и пароль, в том числе данные другого пользователя.

После logout был повторно проверен `test.html`.

Результат:

```text
oUserInit.access = true
oUserInit.error_code =
oUserInit.auth_type = cookie
oUserInit.auth_login =
Env.curUserID =
Env.curUser =
Request.AuthUserID =
```

Главное отличие от старого поведения:

Было до исправления:

```text
auth_type = basic
auth_login = user1
Env.curUserID = 6148914691236517121
Request.AuthUserID = 6148914691236517121
```

Стало после исправления:

```text
auth_type = cookie
auth_login =
Env.curUserID =
Request.AuthUserID =
```

То есть фактического авторизованного пользователя после logout больше нет.

---

## Важный вывод про `oUserInit.access`

После исправления был получен результат:

```text
oUserInit.access = true
auth_type = cookie
auth_login =
Env.curUserID =
Request.AuthUserID =
```

Это важный нюанс.

`oUserInit.access = true` не всегда означает, что пользователь авторизован.

В данном случае:

```text
access = true
```

при пустых:

```text
Env.curUserID
Request.AuthUserID
```

означает, что доступ к портальной странице/сценарию в cookie/form-based режиме разрешен, но пользователь еще не определен.

Поэтому для кастомных API нельзя проверять только:

```js
oUserInit.access
```

Нужно проверять фактическое наличие текущего пользователя.

Лучше использовать:

```js
Env.curUserID
```

или:

```js
Request.AuthUserID
```

---

## Что важно для кастомных API

Для API-хендлеров в WebTutor / WebSoft HCM важно не путать:

```text
доступ к странице/сценарию
```

и

```text
наличие авторизованного пользователя
```

Рекомендуемая проверка:

```html
<%
oUserInit = tools_web.user_init(Request, Request.Query);
Env = Request.Session.GetOptProperty("Env", ({}));
curUserID = Env.GetOptProperty("curUserID", null);

if (curUserID == null || curUserID == "")
{
    Response.SetRespStatus(401, "");
    Response.Write('{"status":"ERROR","message":"Unauthorized"}');
    Cancel();
}
%>
```

Можно дополнительно проверять:

```js
Request.AuthUserID
```

Пример более строгой проверки:

```html
<%
oUserInit = tools_web.user_init(Request, Request.Query);
Env = Request.Session.GetOptProperty("Env", ({}));

curUserID = Env.GetOptProperty("curUserID", null);

if ((curUserID == null || curUserID == "") && (Request.AuthUserID == null || Request.AuthUserID == ""))
{
    Response.SetRespStatus(401, "");
    Response.Write('{"status":"ERROR","message":"Unauthorized"}');
    Cancel();
}
%>
```

Важный вывод:

```text
oUserInit.access полезен для диагностики, но не является достаточным признаком авторизованного пользователя.
```

---

## Что не помогло

В ходе диагностики были проверены несколько направлений.

### 1. Изменение `Authentication.Exclusions` в `xhttp_config.json`

Удаление `/default` из `Exclusions` не решило проблему.

Вывод:

```text
Причина была не в /default.
```

### 2. Проверка кастомного logout-файла

Кастомный logout-файл с редиректом на `logout2.htm` не выполнялся при запросе `/logout`.

Вывод:

```text
/logout обрабатывался встроенным xHTTP logout.
```

### 3. Попытка смотреть только cookie

До исправления logout действительно удалял cookie:

```text
x-auth-id
user_login
user_password
```

Но это не помогало, потому что браузер продолжал отправлять Basic Authorization.

Вывод:

```text
При Basic Auth нужно смотреть не только Cookie, но и заголовок Authorization.
```

### 4. Гипотеза про `/admin`

Обсуждалась версия, что Basic мог появляться после открытия административного интерфейса `/admin`.

Проверка показала, что проблема воспроизводилась даже в чистом браузере без открытия `/admin`.

Вывод:

```text
/admin не был причиной данного кейса.
```

---

## Диагностический чек-лист

Если после logout пользователь снова входит без ввода логина и пароля, нужно проверить следующее.

### 1. Проверить Network-запрос после logout

Особенно заголовки:

```http
Authorization
Cookie
Set-Cookie
Location
```

Если после logout есть:

```http
Authorization: Basic ...
```

то пользователь не является анонимным.

### 2. Проверить ответ `/logout`

Если ответ такой:

```http
HTTP/1.1 302 Found
Location: /loggedoff.html
Set-Cookie: x-auth-id=rm; expires=...
```

значит, скорее всего, работает встроенный xHTTP logout из `xhttp_config.json`.

### 3. Проверить, выполняется ли кастомный logout-файл

Добавить диагностический заголовок:

```js
Response.AddHeader("X-Debug-Logout-Handler", "custom-logout-file-v1");
```

Если заголовка нет в response headers, файл не выполняется.

### 4. Проверить `tools_web.user_init`

Диагностический вывод:

```html
<%
oUserInit = tools_web.user_init(Request, Request.Query);
Env = Request.Session.GetOptProperty("Env", ({}));

Response.Write("<pre>");
Response.Write("oUserInit.access = " + oUserInit.GetOptProperty("access", "") + "\n");
Response.Write("oUserInit.error_code = " + oUserInit.GetOptProperty("error_code", "") + "\n");
Response.Write("oUserInit.auth_type = " + oUserInit.GetOptProperty("auth_type", "") + "\n");
Response.Write("oUserInit.auth_login = " + oUserInit.GetOptProperty("auth_login", "") + "\n");
Response.Write("Env.curUserID = " + Env.GetOptProperty("curUserID", "") + "\n");
Response.Write("Request.AuthUserID = " + Request.AuthUserID + "\n");
Response.Write("</pre>");
%>
```

Критический признак проблемы:

```text
auth_type = basic
auth_login = старый пользователь
Request.AuthUserID заполнен
```

после logout.

### 5. Проверить файл `loggedoff.html`

Если он показывает страницу:

```text
Вы успешно вышли из системы.
Теперь Вы можете закрыть страницу.
```

нужно посмотреть условие внутри файла.

Если эта страница показывается только для:

```text
portal_auth_type = basic
```

или:

```text
portal_auth_type = ntlm
```

значит проблема может быть в типе авторизации узла.

### 6. Проверить карточку узла

Открыть:

```text
Системное администрирование → Узлы
```

Найти узел для текущего хоста, например:

```text
localhost
```

Проверить поле:

```text
Тип авторизации
```

Для возможности повторного ввода логина и пароля после logout на локальном стенде должен использоваться:

```text
cookie / Form-Based
```

---

## Ключевые слова для поиска в документации и комьюнити

Полезные ключевые слова:

```text
Узел
Карточка узла
Тип авторизации
Тип авторизации узла
Аутентификация через узел
cookie / Form-Based
portal_auth_type
curHost.portal_auth_type
tools_web.user_init
auth_type basic
auth_type cookie
x-auth-id
LogoutPath
RedirectLoggedOffUri
loggedoff.html
Authorization Basic после logout
WebTutor logout basic
WebSoft HCM logout basic auth
WebSoft HCM cookie Form-Based
WebTutor cookie Form-Based
Request.AuthUserID
Request.DropSession
tools_web.set_cookie_auth
```

Полезный вопрос для комьюнити:

```text
WebSoft HCM 2023.2.967. После logout xHTTP удаляет x-auth-id/user_login/user_password и редиректит на /loggedoff.html, но tools_web.user_init после выхода возвращает auth_type=basic и снова авторизует старого пользователя. Оказалось, что узел был настроен на Basic. Для локального портала смена типа авторизации узла на cookie / Form-Based решила проблему. Есть ли дополнительные обязательные переменные для cookie/Form-Based в карточке узла или достаточно выбора типа авторизации?
```

---

## Финальный краткий вывод

Проблема logout была вызвана не кодом logout и не `xhttp_config.json`.

Корневая причина:

```text
Узел localhost был настроен на Basic-авторизацию.
```

При Basic Auth браузер сохранял credentials и продолжал отправлять:

```http
Authorization: Basic ...
```

даже после logout.

Встроенный `/logout` корректно удалял cookie:

```text
x-auth-id
user_login
user_password
```

но не мог очистить Basic Authorization, потому что это не cookie.

Решение:

```text
В карточке узла изменить тип авторизации на cookie / Form-Based.
```

После этого logout начал приводить пользователя к форме ввода логина и пароля, а `tools_web.user_init` после выхода перестал возвращать старого пользователя.

Дополнительный важный вывод для API:

```text
Нельзя проверять только oUserInit.access.
Для определения авторизованного пользователя нужно проверять Env.curUserID и/или Request.AuthUserID.
```
