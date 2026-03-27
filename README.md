# Performance Tests (Load Testing)

## **English** | **[Русский](docs/README_RU.md)**

This project implements performance tests for
the [Performance QA Engineer Course](https://github.com/Nikita-Filonov/performance-qa-engineer-course) stand — a
full-featured educational banking system designed for testing and performance validation in training environments. The
platform includes services such as `Kafka`, `Redis`, `PostgreSQL`, `MinIO`, `Grafana`, `Prometheus` and exposes its API
via both HTTP and gRPC protocols.

[![Performance tests](https://github.com/lobanov-qa/performance-tests/actions/workflows/performance-tests.yml/badge.svg)](https://github.com/lobanov-qa/performance-tests/actions/workflows/performance-tests.yml)

---

**Technologies used**:

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

Performance tests are written in **Python** using **Locust** and follow modern software engineering principles like
**SOLID**, **DRY**, and **KISS**. They are designed to simulate realistic business flows and provide visibility into
system performance under load.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Best Practices](#best-practices)
- [Getting Started](#getting-started)
- [Running Performance Tests](#running-performance-tests)
- [Monitoring & Observability](#monitoring--observability)
- [CI/CD](#cicd)
- [Project Structure](#project-structure)
- [Contacts](#contacts)

---

## Project Overview

This performance testing framework supports both **HTTP** and **gRPC** APIs using a unified test structure.

Key components:

- **Scenarios**: Represent realistic user flows, implemented via Locust user classes.
- **API Clients**: Custom reusable HTTP/gRPC clients located in [clients/http/](./clients/http)
  and [clients/grpc/](./clients/grpc), independent of Locust internals.
- **Seeding**: Automated test data generation via a flexible [seeding builder](./seeds/builder.py), triggered through
  Locust event hooks based on the active [scenario plan](./seeds/schema/plan.py).
- **Tools**: Includes generators for fake data, base configurations, and shared Locust user logic.
- **Reporting**: Built-in HTML reports for Locust runs; Prometheus and Grafana metrics available via the course test
  stand.

Supported business scenarios include:

- Existing user: make purchase, get documents, issue virtual card, view operations
- New user: create account, top up card, issue physical card, retrieve account list and documents

---

## Best Practices

This project follows industry-standard best practices:

- **SOLID** design principles for maintainable client architecture
- **DRY** approach to avoid duplication across protocols
- **KISS** philosophy to keep scenarios readable and focused
- Flexible structure to support both HTTP and gRPC testing
- Reusable API clients, designed to be composable and injectable
- The framework is easy to extend with new scenarios or client implementations as the system evolves

---

## Getting Started

> ⚠️ **Important:** this project tests the educational platform [performance-qa-engineer-course](https://github.com/Nikita-Filonov/performance-qa-engineer-course) which must be running locally

### 1. Clone the Repository

```bash
git clone https://github.com/lobanov-qa/performance-tests.git
cd performance-tests
```

### 2. Create a Virtual Environment

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

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

---

## Running Performance Tests

Each scenario can be launched via its own configuration file. The report will be automatically saved in the same directory.

Example:

```bash
locust --config=./scenarios/http/gateway/existing_user_get_documents/v1.0.conf
```

After test execution, open the generated HTML report: `./scenarios/http/gateway/existing_user_get_documents/report.html`

---

## Monitoring & Observability

In addition to built-in Locust reports, system-level metrics can be explored via:

- **Grafana:** http://localhost:3002
- **Prometheus:** http://localhost:9090

These dashboards are preconfigured in the [course infrastructure repository](https://github.com/Nikita-Filonov/performance-qa-engineer-course).

---

## CI/CD

GitHub Actions integration is enabled for this project. You can execute scenarios in headless mode and publish reports to GitHub Pages automatically.

Configuration can be found in [.github/workflows/performance-tests.yml](./.github/workflows/performance-tests.yml).

---

## Project Structure
```
performance-tests/
├── clients/                     # API clients for HTTP and gRPC
│   ├── http/                    # HTTP clients (gateway, accounts, cards, ...)
│   └── grpc/                    # gRPC clients with interceptors for Locust
├── scenarios/                   # Load scenarios (HTTP and gRPC)
│   ├── http/                    # HTTP scenarios (existing_user, new_user)
│   └── grpc/                    # gRPC scenarios
├── seeds/                       # Test data generation (seeding)
│   ├── builder.py               # Builder for data preparation
│   ├── scenario.py              # Scenario logic for seeding
│   └── schema/                  # Data schemas (plan, result)
├── tools/                       # Helper utilities
│   ├── config/                  # Configurations for HTTP/gRPC/Locust
│   ├── locust/                  # Base Locust user classes
│   ├── fakers.py                # Fake data generation
│   ├── logger.py                # Logging
│   └── routes.py                # API routes
├── dumps/                       # Data dumps for seeding
├── .github/workflows/           # CI/CD pipelines
├── docker-compose.load-testing-hub.yaml # Load Testing Hub configuration
├── requirements.txt
└── README.md
```
---
## Contacts

Ready for code review, discussion of solutions and feedback.  
Looking for an opportunity to start a career as an **AQA engineer** to grow in a team and contribute to software quality.

- **GitHub**: [lobanov-qa](https://github.com/lobanov-qa)
- **Telegram**: [lobanov_e_i](https://t.me/lobanov_e_i)
- **LinkedIn**: [evgenii-lobanov-qa](https://www.linkedin.com/in/evgenii-lobanov-qa/)
- **E-mail**: [evgenii-lobanov-qa@yandex.ru](mailto:evgenii-lobanov-qa@yandex.ru)