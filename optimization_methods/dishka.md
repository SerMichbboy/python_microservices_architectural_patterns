# 🧩 Dishka Manifest: Рекомендации по внедрению DI-контейнера в Python-проект

## 📘 Общее описание

**Dishka** — это лёгкий и современный Dependency Injection (DI) контейнер для Python, созданный для управления зависимостями в асинхронных и многослойных приложениях. Он помогает избавиться от ручной передачи зависимостей, упрощает архитектуру и делает код более читаемым и тестируемым.

Dishka особенно хорошо работает с фреймворками:
- FastAPI
- aiohttp
- Telegram bots

---

## 🎯 Цели библиотеки

- ✅ Централизация и автоматизация создания зависимостей
- ✅ Поддержка асинхронных объектов и их финализация
- ✅ Упрощение тестирования через подмену зависимостей
- ✅ Чистая интеграция с современными Python-фреймворками
- ✅ Снижение связности между слоями приложения

---

## 🛠 Рекомендации по внедрению

### 📦 Где размещать контейнер

Создай файл `infrastructure/di.py` — это будет точка входа для всех зависимостей:

```python
from dishka import Provider, Scope
from infrastructure.db.mongo import get_mongo_client
from infrastructure.kafka.producer import get_kafka_producer

provider = Provider(scope=Scope.APP)
provider.provide(get_mongo_client)
provider.provide(get_kafka_producer)
provider.provide(get_db_session)
```

## 🧠 Как регистрировать зависимости
Функции, которые создают объекты, должны быть асинхронными и возвращать готовые экземпляры:

```python
from typing import AsyncGenerator
from sqlalchemy.ext.asyncio import AsyncSession
from contextlib import asynccontextmanager
from app.core.db.db_init import SessionFactory

@asynccontextmanager
async def get_db_session() -> AsyncGenerator[AsyncSession, None]:
    async with SessionFactory() as session:
        yield session
```

## 🧱 Архитектурный шаблон (TDD + Clean Architecture)

```python
  src/
├── api/                # Входные точки: роуты, сериализация
│   ├── v1/
│   └── routers/
│   └── schemas/
│
├── application/        # Use-cases, бизнес-логика
│   ├── services/
│   └── interfaces/
│
├── domain/             # Сущности, value objects
│   ├── models/
│   └── schemas/
│
├── infrastructure/     # Реализация интерфейсов: БД, Kafka, Redis
│   ├── db/
│   ├── kafka/
│   ├── config.py
│   └── di.py
```

## 💡 Примеры лучших практик
### 🔧 Примеры Инъекции в сервис


```python
from dishka import inject, FromDishka
from motor.motor_asyncio import AsyncIOMotorClient
from aiokafka import AIOKafkaProducer

@inject
class UserService:
    def __init__(
        self,
        db: AsyncIOMotorClient = FromDishka(),
        kafka: AIOKafkaProducer = FromDishka()
    ):
        self.db = db
        self.kafka = kafka

```

```python
# application/services/user_service.py
from dishka import inject, FromDishka
from sqlalchemy.ext.asyncio import AsyncSession

@inject
class UserService:
    def __init__(self, session: AsyncSession = FromDishka()):
        self.session = session

    async def get_user(self, user_id: int):
        result = await self.session.execute(
            select(UserModel).where(UserModel.id == user_id)
        )
        return result.scalar_one_or_none()
```


### 🚀 Интеграция с FastAPI


```python
from fastapi import FastAPI
from dishka.integrations.fastapi import setup_dishka, DishkaRoute
from infrastructure.di import provider

app = FastAPI()
setup_dishka(app, provider=provider, route_class=DishkaRoute)

```

### 🧪 Тестирование с подменой зависимостей

```python
from dishka import Provider

test_provider = Provider()
test_provider.provide(lambda: MockKafkaProducer())
test_provider.provide(lambda: FakeMongoClient())

# Используй test_provider в тестовом FastAPI-приложении
```

## 📘 Статья на Хабре: "Год с Dishka" — https://habr.com/ru/articles/894286/

## 📦 Официальный GitHub Dishka — https://github.com/reagento/dishka
