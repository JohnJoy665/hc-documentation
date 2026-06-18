---
title: "Агент создания сотрудников"
topic: "agents/collaborators/create"
source_files:
  - "raw/2026-06-17/agents/create-collaborators-agent-bind-to-db-experiment.md"
  - "raw/2026-06-17/code-libraries/tools-new-doc-by-name.md"
  - "raw/2026-06-17/catalogs/collaborators-schema-and-document-example.md"
status: "draft"
---

# Агент создания сотрудников

## Кратко

Проверенный пользователем рабочий способ создания сотрудников в агенте:

1. Создать новый документ через `tools.new_doc_by_name('collaborator', false)`.
2. Сразу связать документ с текущей базой через `docUser.BindToDb(DefaultDb)`.
3. Заполнить `docUser.TopElem`.
4. Вызвать `docUser.Save()`.

Серверный код агента выполняется внутренним серверным языком WebTutor.

## Рабочий фрагмент

```js
docUser = tools.new_doc_by_name( 'collaborator', false);
docUser.BindToDb( DefaultDb );
teUser = docUser.TopElem;

teUser.lastname = nameParts[0];
teUser.firstname = nameParts[1];
teUser.middlename = nameParts[2];

teUser.login = "user" + curN;
teUser.password = "user" + curN;

docUser.Save();
```

## Полный рабочий код из эксперимента

```js
LOG_NAME = "newUsers";

EnableLog(LOG_NAME, true);

newUsers = [
    "Соколов Артем Игоревич", "Морозова Анна Сергеевна", "Кузнецов Дмитрий Павлович",
    "Васильева Мария Олеговна", "Новиков Кирилл Андреевич", "Федорова Елена Викторовна",
    "Павлов Максим Денисович", "Семенова Ольга Романовна", "Голубев Илья Алексеевич",
    "Виноградова Дарья Константиновна", "Белов Никита Михайлович", "Комарова Ирина Валерьевна",
    "Орлов Роман Евгеньевич", "Киселева Наталья Ильинична", "Макаров Алексей Петрович",
    "Захарова Виктория Антоновна", "Андреев Тимур Станиславович", "Тарасова Полина Дмитриевна",
    "Борисов Глеб Аркадьевич", "Егорова Светлана Николаевна", "Матвеев Руслан Олегович",
    "Никитина Юлия Максимовна", "Соловьев Павел Игоревич", "Крылова Алиса Андреевна",
    "Григорьев Арсений Владимирович", "Романова Ксения Сергеевна", "Степанов Лев Александрович",
    "Фомина Милана Ильинична", "Алексеев Ярослав Денисович", "Давыдова Вероника Павловна",
    "Михайлов Марк Романович", "Баранова Екатерина Олеговна", "Фролов Данил Викторович",
    "Белкина Софья Артемовна", "Игнатьев Матвей Кириллович", "Савельева Анастасия Евгеньевна",
    "Котов Егор Валерьевич", "Лазарева Маргарита Алексеевна", "Сидоров Иван Константинович",
    "Полякова Валерия Михайловна", "Титов Мирон Сергеевич", "Калашникова Арина Денисовна",
    "Ершов Богдан Павлович", "Назарова Таисия Романовна", "Абрамов Владислав Олегович",
    "Гусева Диана Викторовна", "Жуков Артур Ильич", "Мельникова Любовь Андреевна",
    "Лебедев Савелий Петрович", "Панина Ева Максимовна"
];

existingUsers = ArraySelectAll(XQuery("for $elem in collaborators return $elem"));
curN = ArrayCount(existingUsers) + 1;

for (fullname in newUsers)
{
    try
    {
        nameParts = fullname.split(" ");

        docUser = tools.new_doc_by_name( 'collaborator', false);
        docUser.BindToDb( DefaultDb );
        teUser = docUser.TopElem;

        teUser.lastname = nameParts[0];
        teUser.firstname = nameParts[1];
        teUser.middlename = nameParts[2];

        teUser.login = "user" + curN;
        teUser.password = "user" + curN;

        docUser.Save();

        LogEvent(LOG_NAME, "Пользователь " + fullname + " создан");
    }
    catch (err)
    {
        LogEvent(LOG_NAME, "Ошибка при создании пользователя " + fullname + ": " + err);
    }

    curN = curN + 1;
}
```

## Ошибки из эксперимента

### Empty document url

Вариант без `BindToDb(DefaultDb)`:

```js
docUser = tools.new_doc_by_name("collaborator", false);
docUser.Save();
```

дал ошибку:

```text
Ошибка при создании пользователя ...: Empty document url
(Save(),  x-local://wtv/wtv_server_agent.xmd,   line 316)
```

Причина по описанию `BindToDb`: при создании нового документа нужно связать его с базой данных. Если документ не связан с базой данных, выводится ошибка `Empty document url`.

### Unknown object property: lastname

Вариант:

```js
docUser = tools.new_doc_by_name("collaborator", true);
```

дал ошибку:

```text
Ошибка при создании пользователя ...: Unknown object property: lastname
(x-local://wtv/wtv_server_agent.xmd,   line 309)
```

Вывод: для создания объекта сотрудника в этом сценарии нужен второй аргумент `false`.

## Источники

- `raw/2026-06-17/agents/create-collaborators-agent-bind-to-db-experiment.md`
- `raw/2026-06-17/code-libraries/tools-new-doc-by-name.md`
- `raw/2026-06-17/catalogs/collaborators-schema-and-document-example.md`
