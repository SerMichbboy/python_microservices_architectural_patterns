---

# 📘 About "Architect patterns of microservices: recomendations"

I started this project just for myself, to document all the knowledge about microservice architecture that I’ve encountered and seen great potential for using in future projects.

All documentation is structured in .md format and focuses purely on standalone technical insights.

I'm glad if you find something useful or interesting here.

The project is open to suggestions and conversations.

When I started, I wrote in Russian for convenience, but future updates will be in English.

# Welcome

Этот репозиторий создан как **справочник архитектурных решений** и поиска лучших практик при построении систем на Python.  
Здесь собраны **паттерны, рекомендации по технологиям, правила кодирования и DevOps-подходы**, которые помогают создавать масштабируемые, надёжные и поддерживаемые продукты.

---

## 📚 Что здесь можно найти

- 🔹 **Архитектурные паттерны** — API Gateway, BFF, Saga, Circuit Breaker и др.  
- 🔹 **Взаимодействие сервисов** — синхронное (REST/gRPC), асинхронное (Kafka, RabbitMQ).  
- 🔹 **Стратегии управления данными** — Database per Service, Event Sourcing, CQRS.  
- 🔹 **Надёжность** — Retry, Timeout, Bulkhead, Sidecar.  
- 🔹 **Методы разработки** — DDD, TDD, PDD
- 🔹 **Рекомендации по фреймворкам** — FastAPI, Django, SQLAlchemy, Celery.  
- 🔹 **DevOps и CI/CD практики** — Docker, Kubernetes, Jenkins/GitHub Actions.  

---

## 📑 Формат документации

Каждый паттерн или метод описывается в виде таблицы:

`Категория | Паттерн | Описание | Преимущества | Недостатки`

Например:

| Категория                 | Паттерн              | Описание                                    | Преимущества                              | Недостатки                              |
|----------------------------|----------------------|---------------------------------------------|--------------------------------------------|------------------------------------------|
| Композиция сервисов        | API Gateway          | Все запросы идут через один входной сервис | Централизованная маршрутизация, кэширование | Точка отказа, нужна высокая доступность |
| Взаимодействие сервисов    | gRPC/HTTP (синхрон) | Сервисы напрямую вызывают друг друга        | Простая отладка, предсказуемое поведение   | Повышенная связность                    |

---

## 🎯 Цель проекта

- Систематизировать архитектурные решения.  
- Упростить выбор подхода при проектировании систем.  
- Стандартизировать разработку, тестирование и деплой.  
- Поддерживать **живую базу знаний**, актуальную для Python-проектов.  

---

## 🚀 Как использовать

1. Открывай нужный раздел.  
2. Изучи примеры и плюсы/минусы подходов.  
3. Применяй в своих проектах или используй как шпаргалку.  

---

## 📌 Планы развития

- Добавить больше паттернов (CQRS, Event Sourcing).  
- Расширить разделы по DevOps и CI/CD.  
- Подготовить примеры кода для каждого паттерна.  
- Сделать схемы (диаграммы C4, BPMN).  

---

✍️ Автор: [SerMichbboy](https://github.com/SerMichbboy)  
📖 Репозиторий открыт для дополнений и pull-request'ов.
