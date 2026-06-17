---
title: "Локальный учебный контур WebSoft HCM на PostgreSQL"
topic: "local-dev-contour/postgresql"
source_files:
  - "raw/2026-06-17/local-postgresql/local-study-contour-wt-postgresql.txt"
  - "raw/2026-06-17/install-pack/websoft-hcm-install-pack/docs/14-local-postgresql-study-stand.md"
status: "draft"
---

# Локальный учебный контур WebSoft HCM на PostgreSQL

## Кратко

В локальном эксперименте был создан отдельный учебный контур WebSoft HCM на PostgreSQL без изменения основной локальной установки.

Основная папка:

```text
C:\WebSoft\WebSoftServer
```

Учебная копия:

```text
C:\WebSoft\WebSoftServer_PG_LOCAL_STUDY
```

## Параметры PostgreSQL

На компьютере уже был установлен PostgreSQL 16.2. Служба:

```text
postgresql-x64-16
```

Клиент:

```text
C:\Program Files\PostgreSQL\16\bin\psql.exe
```

Для учебного стенда созданы:

```text
Database: websoft_hcm_local_study
User: websoft_hcm_local_study_user
Password: websoft_hcm_local_study_pass
Host: localhost
Port: 5432
```

## Переключение типа базы

В копии портала тип базы был изменен через `x-shell`.

Попытка использовать значение `pgsql` была отклонена: x-shell сообщил, что допустимые значения:

```text
xml
mssql
postgresql
oracle
```

Корректная команда:

```text
db set type postgresql
```

После ввода параметров подключения x-shell создал безопасную копию `spxml_unibridge_config.xml` и сообщил:

```text
Database Type postgresql set, xhttp restart recommended.
```

## Инициализация и миграция

После запуска `xhttp_64.exe` из учебной копии была выполнена инициализация:

```text
db init
```

В процессе появлялись предупреждения и ошибка загрузки внутреннего документа `x-local://wtv/wtv_view_main.xml`, но итоговый статус был успешным:

```text
Task 'InitializeDatabase' succeeded.
```

Затем выполнена миграция из XML-хранилища:

```text
db migrate from xml
```

Итог:

```text
File System Migration completed.
Overall Docs:21627.
Migration Errors:0.
Task 'MigrateFileSystemTables' succeeded.
```

После миграции выполнена перестройка каталогов и индексов:

```text
rctg start
```

В процессе были сообщения об отсутствующей библиотеке `libAnalytics` и несколько ошибок `ProcessDocInCatalog() failed` по ресурсам, но задача завершилась:

```text
Indexes rebuilt
Task finished
```

## Запуск

Для запуска учебного портала нужно запускать сервер из папки копии:

```text
C:\WebSoft\WebSoftServer_PG_LOCAL_STUDY
.\xhttp_64.exe
```

Для входа в портал использовалась демонстрационная учетная запись:

```text
user1 / user1
```

## Проверенные факты

- Основная папка `C:\WebSoft\WebSoftServer` не изменялась.
- Данные были перенесены из исходного XML-хранилища в PostgreSQL.
- Миграция перенесла 21627 документов без ошибок миграции.
- Таблицы появились и доступны для изучения через DBeaver.

## TODO / вопросы для проверки

- TODO: нужно проверить влияние ошибок `ProcessDocInCatalog() failed` на поиск и каталоги.
- TODO: нужно проверить причину отсутствия `libAnalytics`.
- TODO: нужно описать безопасный rollback для учебного контура.

## Источники

- `raw/2026-06-17/local-postgresql/local-study-contour-wt-postgresql.txt`
