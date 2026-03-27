# Performance Tests (Нагрузочное тестирование)

## **[English](../README.md)** | **Русский**

Этот проект реализует нагрузочное тестирование для [стенда курса Performance QA Engineer](https://github.com/Nikita-Filonov/performance-qa-engineer-course) — полнофункциональной образовательной банковской системы, предназначенной для тестирования и проверки производительности в учебных средах. Платформа включает такие сервисы, как `Kafka`, `Redis`, `PostgreSQL`, `MinIO`, `Grafana`, `Prometheus` и предоставляет свой API через протоколы HTTP и gRPC.


[![Performance tests](https://github.com/lobanov-qa/performance-tests/actions/workflows/performance-tests.yml/badge.svg)](https://github.com/lobanov-qa/performance-tests/actions/workflows/performance-tests.yml)

---

**Используемые технологии**:

![Python](https://img.shields.io/badge/python-3670A0?style=flat-square&logo=python&logoColor=ffdd54)
![Locust](https://img.shields.io/badge/-locust-%2300B075?style=flat-square&logo=locust&logoColor=white)
![Pydantic](https://img.shields.io/badge/-pydantic-%23E92063?style=flat-square&logo=pydantic&logoColor=white)
![gRPC](https://img.shields.io/badge/-gRPC-%23009688?style=flat-square&logo=grpc&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Redis](https://img.shields.io/badge/-redis-%23DC382D?style=flat-square&logo=redis&logoColor=white)
![Kafka](https://img.shields.io/badge/-kafka-%23231F20?style=flat-square&logo=apachekafka&logoColor=white)
![MinIO](https://img.shields.io/badge/MinIO-FF0000?style=flat-square&logo=minio&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/postgres-%23316192?style=flat-square&logo=postgresql&logoColor=white)
![Grafana](https://img.shields.io/badge/-grafana-%23F46800?style=flat-square&logo=grafana&logoColor=white)
![Prometheus](https://img.shields.io/badge/-prometheus-%23E6522C?style=flat-square&logo=prometheus&logoColor=white)
![Kibana](https://img.shields.io/badge/Kibana-005571?style=flat-square&logo=kibana&logoColor=white)

Тесты производительности написаны на **Python** с использованием **Locust** и следуют современным принципам разработки ПО, таким как **SOLID**, **DRY** и **KISS**. Они предназначены для моделирования реалистичных бизнес-сценариев и предоставления видимости производительности системы под нагрузкой.

---

## Содержание

- [Обзор проекта](#обзор-проекта)
- [Лучшие практики](#лучшие-практики)
- [Начало работы](#начало-работы)
- [Запуск нагрузочных тестов](#запуск-нагрузочных-тестов)
- [Мониторинг и наблюдаемость](#мониторинг-и-наблюдаемость)
- [CI/CD](#cicd)
- [Структура проекта](#структура-проекта)
- [Контакты](#контакты)

---

## Обзор проекта

Этот фреймворк для нагрузочного тестирования поддерживает как **HTTP**, так и **gRPC** API, используя унифицированную структуру тестов.

Ключевые компоненты:

- **Сценарии**: Представляют реалистичные пользовательские потоки, реализованные через классы пользователей Locust.
- **API Клиенты**: Переиспользуемые HTTP/gRPC клиенты, расположенные в [clients/http/](./clients/http) и [clients/grpc/](./clients/grpc), независимые от внутренней реализации Locust.
- **Сиддинг**: Автоматическая генерация тестовых данных через гибкий [сборщик сиддинга](./seeds/builder.py), запускаемый через хуки событий Locust на основе активного [плана сценария](./seeds/schema/plan.py).
- **Инструменты**: Включает генераторы фейковых данных, базовые конфигурации и общую логику пользователей Locust.
- **Отчётность**: Встроенные HTML-отчёты для запусков Locust; метрики Prometheus и Grafana доступны через учебный стенд курса.

Поддерживаемые бизнес-сценарии включают:

- Существующий пользователь: совершить покупку, получить документы, выпустить виртуальную карту, просмотреть операции
- Новый пользователь: создать аккаунт, пополнить карту, выпустить физическую карту, получить список счетов и документов

---

## Лучшие практики

Этот проект следует отраслевым стандартам лучших практик:

- Принципы проектирования **SOLID** для поддерживаемой архитектуры клиентов
- Подход **DRY** для избежания дублирования между протоколами
- Философия **KISS** для сохранения сценариев читаемыми и сфокусированными
- Гибкая структура для поддержки тестирования как HTTP, так и gRPC
- Переиспользуемые API клиенты, спроектированные как композируемые и инжектируемые
- Фреймворк легко расширяется новыми сценариями или реализациями клиентов по мере развития системы

---

## Начало работы

> ⚠️ **Важно:** проект тестирует учебную  платформу [performance-qa-engineer-course](https://github.com/Nikita-Filonov/performance-qa-engineer-course) которая должна быть запущенна локально

### 1. Клонирование репозитория

```bash
git clone https://github.com/lobanov-qa/performance-tests.git
cd performance-tests
```

### 2. Создание виртуального окружения

#### Linux / MacOS

```bash
python3 -m venv venv
source venv/bin/activate
```

#### Windows

```bash
python -m venv venv
venv\Scripts\activate
```

### 3. Установка зависимостей

```bash
pip install -r requirements.txt
```

---

## Запуск нагрузочных тестов

Каждый сценарий может быть запущен через свой собственный файл конфигурации. Отчёт будет автоматически сохранён в той же директории.

Пример:

```bash
locust --config=./scenarios/http/gateway/existing_user_get_documents/v1.0.conf
```

После выполнения теста откройте сгенерированный HTML-отчёт: `./scenarios/http/gateway/existing_user_get_documents/report.html`

---

## Мониторинг и наблюдаемость

В дополнение к встроенным отчётам Locust, метрики на уровне системы могут быть исследованы через:

- **Grafana:** http://localhost:3002
- **Prometheus:** http://localhost:9090

Эти дашборды предварительно настроены в [репозитории инфраструктуры курса](https://github.com/Nikita-Filonov/performance-qa-engineer-course).

---

## CI/CD

Интеграция с GitHub Actions включена для этого проекта. Вы можете выполнять сценарии в headless-режиме и автоматически публиковать отчёты на GitHub Pages.

Конфигурация находится в [.github/workflows/performance-tests.yml](./.github/workflows/performance-tests.yml).


## Структура проекта
```
performance-tests/
├── clients/                     # API-клиенты для HTTP и gRPC
│   ├── http/                    # HTTP-клиенты (gateway, accounts, cards, ...)
│   └── grpc/                    # gRPC-клиенты с интерсепторами для Locust
├── scenarios/                   # Сценарии нагрузки (HTTP и gRPC)
│   ├── http/                    # HTTP-сценарии (existing_user, new_user)
│   └── grpc/                    # gRPC-сценарии
├── seeds/                       # Генерация тестовых данных (сидинг)
│   ├── builder.py               # Билдер для подготовки данных
│   ├── scenario.py              # Логика сценариев для сидинга
│   └── schema/                  # Схемы данных (plan, result)
├── tools/                       # Вспомогательные утилиты
│   ├── config/                  # Конфигурации для HTTP/gRPC/Locust
│   ├── locust/                  # Базовые классы пользователей Locust
│   ├── fakers.py                # Генерация фейковых данных
│   ├── logger.py                # Логирование
│   └── routes.py                # Маршруты API
├── dumps/                       # Дампы данных для сидинга
├── .github/workflows/           # CI/CD пайплайны
├── docker-compose.load-testing-hub.yaml #Load Testing Hub 
├── requirements.txt
└── README.md
```
---
## Контакты

Готов к код-ревью, обсуждению решений и обратной связи.  
Ищу возможность начать карьеру в качестве **инженера AQA**, чтобы расти в команде и вносить вклад в качество ПО.

- **GitHub**: [lobanov-qa](https://github.com/lobanov-qa)
- **Telegram**: [lobanov_e_i](https://t.me/lobanov_e_i)
- **LinkedIn**: [evgenii-lobanov-qa](https://www.linkedin.com/in/evgenii-lobanov-qa/)
- **E-mail**: [evgenii-lobanov-qa@yandex.ru](mailto:evgenii-lobanov-qa@yandex.ru)