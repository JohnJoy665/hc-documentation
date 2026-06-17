---
title: Справочник API v3 (OpenAPI)
source_urls:
  - https://news.websoft.ru/_wt/6735700257609166038
collected_at: 2026-06-11
status: partial
---

# Справочник API v3 (OpenAPI)

API v3 имеет обширную спецификацию OpenAPI, генерируемую автоматически порталом Websoft HCM.  В рамках данного пакета приведены основные сведения (см. `docs/16-api-v3.md`).  Для получения полного справочника используйте файл `swagger.json` или `swagger.yaml`, доступный по адресу `/openapi/hcm/v1/swagger.json`.  В этих файлах описаны:

* Все пути (endpoints) – например, `/employee`, `/course`, `/event` и т.д.
* Операции (`GET`, `POST`, `PATCH`, `DELETE`) и их параметры.
* Модели данных – описание полей и типов объектов.
* Примеры запросов и ответов.

Разработчики могут импортировать OpenAPI‑описание в инструменты Postman, Swagger UI или генераторы клиентского кода (например, OpenAPI Generator), чтобы упростить работу с API.