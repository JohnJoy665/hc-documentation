---
title: API версия 1 (SOAP/XML)
source_urls:
  - https://news.websoft.ru/_wt/6680061488781884204
collected_at: 2026-06-11
status: partial
---

# API версия 1 (SOAP/XML)

Версия 1 API Websoft HCM представляет собой набор SOAP‑веб‑сервисов, предназначенных для работы с объектами платформы.  Этот интерфейс появился в ранних версиях WebTutor (с 2.7) и поддерживает основные операции: создание, чтение, обновление и удаление (CRUD).  Несмотря на появление OpenAPI‑интерфейса, SOAP‑сервисы продолжают использоваться для интеграции со старыми системами.

## Основные особенности

* **Формат XML** – все запросы и ответы формируются в формате XML согласно WSDL‑описанию сервиса.
* **Методы** – каждый сервис содержит методы `Create`, `Get`, `Update` и `Delete`, принимающие набор параметров и возвращающие структуру ответа.
* **Аутентификация** – используется Basic‑Auth (логин/пароль) или передача ключа безопасности в заголовке SOAP.
* **Объекты** – через API можно управлять пользователями, курсами, учебными мероприятиями, элементами каталога и др.

## Пример запроса

Ниже приведён пример запроса SOAP для получения информации о пользователе.  Параметры запроса зависят от конкретного WSDL файла, предоставляемого порталом.

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
                  xmlns:urn="urn:WebTutor">
  <soapenv:Header/>
  <soapenv:Body>
    <urn:GetEmployee>
      <urn:EmployeeId>12345</urn:EmployeeId>
    </urn:GetEmployee>
  </soapenv:Body>
</soapenv:Envelope>
```

Ответ:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
  <soapenv:Body>
    <urn:GetEmployeeResponse>
      <urn:Employee>
        <urn:Id>12345</urn:Id>
        <urn:FirstName>Иван</urn:FirstName>
        <urn:LastName>Иванов</urn:LastName>
        …
      </urn:Employee>
    </urn:GetEmployeeResponse>
  </soapenv:Body>
</soapenv:Envelope>
```

## Использование

1. Скачайте WSDL‑файлы сервиса API версии 1 на портале Websoft HCM (обычно доступны по ссылке вида `/oapi/wsdl`).
2. Сгенерируйте клиентский код (например, используя `wsimport` в Java или `svcutil` в .NET) по WSDL.
3. Настройте аутентификацию (Basic‑Auth) и отправляйте SOAP‑запросы к необходимым методам.
4. Обрабатывайте ответы и выполняйте бизнес‑логику.

> Важно: API v1 считается устаревшим и развивается только для обратной совместимости.  Для новых проектов рекомендуется использовать API v3.