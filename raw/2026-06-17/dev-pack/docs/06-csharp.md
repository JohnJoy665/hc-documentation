---
title: "Разработка на C# в Websoft HCM"
source_urls:
  - "https://news.websoft.ru/_wt/languages/csharp"
collected_at: "2026-06-11"
status: "partial"
---

# Разработка на C#

C# является одним из основных языков для создания расширений в WebSoft HCM. Он используется для серверных агентов, удалённых действий, библиотек и интеграционных модулей. Код C# компилируется в сборку и исполняется в среде .NET Core, что обеспечивает высокую производительность и доступ к широкому набору библиотек.

## Поддерживаемые возможности

- **Удалённые действия.** C#‑класс должен реализовать интерфейс `IWebTutorAction` и определять метод `Execute(object context)`. Через удалённые действия можно вызывать код из бизнес‑процессов, веб‑форм и API.
- **Библиотеки кода.** Для объединения связанных функций создаются классы со статическими методами. Вызов осуществляется через `tools.call_code_library_method("LibName", "MethodName", args)`.
- **Серверные агенты.** Агент на C# реализует интерфейс `IWebTutorAgent` или базовый класс `WebTutorAgent`. Агент выполняет код по расписанию и может работать с файлами, уведомлениями, отчетами.
- **Работа с базой данных.** Через встроенные объекты и ORM разработчик может выполнять запросы, выбирать и обновлять сущности. Для специфичных операций используется XQuery или SQL (для внешних БД).

## Пример удалённого действия

```csharp
using WebSoft.Common;
using WebSoft.API;

public class ChangeUserPassword : IWebTutorAction
{
    public object Execute(object context)
    {
        var args = context as WTActionContext;
        int userId = Convert.ToInt32(args.Parameters["USER_ID"]);
        string newPassword = args.Parameters["NEW_PASSWORD"].ToString();
        tools.change_user_password(userId, newPassword);
        return "Password updated";
    }
}
```

## Пример сервера агента

```csharp
using System;
using WebSoft.Common;

public class DailyReportAgent : WebTutorAgent
{
    public override void Execute()
    {
        var report = tools.generate_daily_report();
        tools.send_notification(tools.get_user(1), "Daily report", report);
    }
}
```

## Настройки компиляции и отладки

* В OmniCode и на сервере присутствует механизм сборки C# с использованием настроек проекта, определённых в файле `omnicode.json`.
* Для подключения сторонних библиотек необходимо разместить файлы `.dll` в каталоге `\components` и добавить ссылки в конфигурации.
* Для отладки в OmniCode можно устанавливать точки останова и использовать консоль вывода для просмотра результатов.

## Полезные советы

- Разграничивайте бизнес‑логику по классам и методам, чтобы код был читаемым и повторно используемым.
- Обрабатывайте исключения и возвращайте информативные сообщения о результатах выполнения.
- Воспользуйтесь встроенными объектами (tools, user, process) для работы с данными портала.

> Важно:
> В исходной документации раздел о C# содержит ссылки на дополнительные материалы и примеры. Для глубокой разработки рекомендуется изучить примеры библиотек и существующие сервисы в папках `wtv/libs` и `components`.