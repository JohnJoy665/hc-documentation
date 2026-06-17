---
title: Установка WebSoft HCM через Docker
source_urls:
  - https://docs.websoft.ru/_wt/67387113511338054212/diparent/69247367959797/watype/6682650392188845894
collected_at: 2026-06-10
status: draft
---

# Установка WebSoft HCM через Docker

Docker — удобный способ быстро развернуть WebSoft HCM и связанные сервисы без сложной настройки окружения. В этом разделе приведены основные шаги для запуска контейнеров и настройки переменных.

> **Важно:** Все команды приводятся в том виде, в каком они представлены в официальной документации. Перед выполнением убедитесь, что Docker и Docker Compose установлены на сервере и имеются права на скачивание образов.

## 1. Скачивание и запуск основного контейнера WebSoft HCM

1. Убедитесь, что на сервере есть свободное место (рекомендуется не менее 16 ГБ) и что открыт порт, на котором будет работать портал (например 80/8080). 
2. Скачайте образ последней версии и запустите контейнер:

```bash
docker pull websoft/webtutor-hcm:latest
docker run -d --name hcm --restart=always \
  -p 8080:80 \
  -v /opt/hcm/data:/usr/share/hcm/data \
  -v /opt/hcm/logs:/usr/share/hcm/logs \
  -e HCM_CONFIG_DB_TYPE=mssql \
  -e HCM_CONFIG_DB_HOST=db.example.com \
  -e HCM_CONFIG_DB_PORT=1433 \
  -e HCM_CONFIG_DB_USER=sa \
  -e HCM_CONFIG_DB_PASSWORD=«пароль» \
  websoft/webtutor-hcm:latest
```

Параметры `HCM_CONFIG_DB_*` указывают тип базы данных (mssql/postgres), хост, порт и учетные данные. Замонтированные каталоги `/opt/hcm/data` и `/opt/hcm/logs` обеспечивают сохранение данных и логов вне контейнера【182237728321055†screenshot】.

## 2. Установка WebSoftIDM в контейнере

WebSoftIDM отвечает за управление пользователями и аутентификацию. Он может быть развернут отдельно или совместно с HCM:

```bash
docker pull websoft/webtutor-idm:latest
docker run -d --name idm --restart=always \
  -p 8081:80 \
  -e IDM_CONFIG_ADMIN_LOGIN=admin \
  -e IDM_CONFIG_ADMIN_PASSWORD=«пароль» \
  -e IDM_CONFIG_PORTAL_HOST=hcm.example.com \
  websoft/webtutor-idm:latest
```

Контейнер требует указать учетную запись администратора IDM и адрес портала HCM, с которым он будет интегрироваться【687277790184942†screenshot】.

## 3. Сервер записи (VClass)

Для записи занятий и вебинаров используется сервер VClass. Документация предлагает два варианта: установка на Ubuntu напрямую или запуск в Docker. Контейнерный вариант выглядит следующим образом【771599920411069†screenshot】:

```bash
docker pull websoft/vclass-recorder:latest
docker run -d --name vclass-recorder --restart=always \
  --device /dev/video0:/dev/video0 \
  -p 8082:80 \
  -e VCLASS_STORAGE_PATH=/var/vclass/records \
  -v /var/vclass/records:/records \
  websoft/vclass-recorder:latest
```

Контейнер монтирует каталог для хранения записей и пробрасывает видеоустройство. В руководстве подчеркнуто, что для полноценной работы сервер должен иметь достаточные ресурсы CPU и SSD диск, так как процесс записи требует высокой производительности【363971634706873†screenshot】.

## 4. Inventa

Inventa — модуль интеллектуального поиска и анализа контента. Для установки используется отдельный образ:

```bash
docker pull websoft/inventa:latest
docker run -d --name inventa --restart=always \
  -p 8083:80 \
  -v /opt/inventa/data:/app/data \
  -e INVENTA_CONFIG_PORTAL_URL=https://hcm.example.com \
  websoft/inventa:latest
```

В переменной `INVENTA_CONFIG_PORTAL_URL` указывается адрес портала WebSoft HCM. После запуска необходимо настроить интеграцию в интерфейсе HCM【848555681754747†screenshot】.

## 5. Telemetry (Signoz)

Для мониторинга производительности и сбора логов предлагается установить систему Signoz. В документации приведён пример `docker-compose.yml`, который запускает сервисы для сбора метрик и отображения данных【452867952766753†screenshot】:

```bash
version: "3.8"
services:
  signoz-db:
    image: signoz/postgres:latest
    volumes:
      - ./signoz-data:/var/lib/postgresql/data
    environment:
      - POSTGRES_USER=signoz
      - POSTGRES_PASSWORD=«пароль»
  signoz:
    image: signoz/frontend:latest
    ports:
      - "3301:3301"
    depends_on:
      - signoz-db
```

Запустите сервис командой `docker compose up -d`. После установки необходимо интегрировать сбор метрик в WebSoft HCM, указав параметры подключения в конфигурационных файлах телеметрии.

## Настройка переменных и хранение данных

Все перечисленные сервисы поддерживают переменные окружения для тонкой настройки (язык интерфейса, пути к логам, параметры БД, адреса внешних сервисов). Рекомендуется хранить данные и логи на внешних томах, чтобы упростить обновление контейнеров и резервное копирование. Более подробные переменные приведены в официальной документации на страницах описания каждого сервиса【182237728321055†screenshot】.
