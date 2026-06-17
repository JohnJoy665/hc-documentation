---
title: Примеры кода Websoft HCM
source_urls:
  - https://news.websoft.ru/_wt/6680061488781884204
  - https://news.websoft.ru/_wt/wiki_base/6735702551142867452/
collected_at: 2026-06-11
status: completed
---

# Примеры кода Websoft HCM

В этом разделе собраны простые примеры, демонстрирующие использование языков и библиотек Websoft HCM.  Примеры предназначены для быстрой ориентации и могут быть расширены в зависимости от ваших задач.

## Преобразование значений с помощью Tools

```javascript
// Использование функции Int для безопасного преобразования строки в целое число
try {
    var number = tools.Int('42');
    // number === 42
} catch (ex) {
    // Ошибка при преобразовании
    tools.log('Не удалось преобразовать строку в число');
}

// Безопасное преобразование – возвращает null при ошибке
var optNumber = tools.OptInt('abc'); // null
```

## XQuery: выборка опубликованных курсов

```xquery
(: Возвращает список курсов со статусом publish :)
for $course in courses
where $course/status = 'publish'
order by $course/name
return $course
```

Данный запрос демонстрирует базовый шаблон XQuery: объявление переменной (`for`), условие (`where`), сортировка (`order by`) и возвращаемое значение【469683879944750†screenshot】.

## Вызов функции из библиотеки кода

```javascript
// Вызов функции sendNotification из библиотеки libMain через API v3
var response = tools.call_code_library_method('libMain', 'sendNotification', {
    employeeId: 12345,
    message: 'Ваш курс доступен для изучения'
});
if (response.status === 'success') {
    tools.log('Уведомление отправлено');
}
```

## Вызов API v3 через curl

```bash
curl -X POST https://portal.example.com/openapi/hcm/v1/functions/sendNotification \
     -H 'X-App-Id: my_app' \
     -u 'api_user:secret' \
     -H 'Content-Type: application/json' \
     -d '{"employeeId":12345,"message":"Добрый день"}'
```

Этот пример показывает, как вызвать высокоуровневую функцию через API v3, передав параметры в формате JSON.

## Фрагмент XAML для кнопки и действия

```xml
<SPXMLScreen>
    <Action Name="ArchiveCourse" Source="libCourseAdmin" Success="OnOk" Failure="OnFail" />
    <Button Caption="Архивировать" Action="ArchiveCourse" />
    <Label Caption="Нажмите кнопку, чтобы перенести курс в архив." />
</SPXMLScreen>
```

Здесь действие `ArchiveCourse` определено в библиотеке `libCourseAdmin`; кнопка вызывает это действие, а подпись информирует пользователя о назначении действия【575597786908733†screenshot】.