# 🧩 Dishka Manifest: Введение в DI-контейнер для Python  

## 📘 Что такое Dishka?  
**Dishka** — это лёгкий и современный Dependency Injection (DI) контейнер для Python.  

Он нужен для того, чтобы:  
- перестать вручную передавать зависимости между функциями и классами;  
- сделать код более понятным и гибким;  
- упростить тестирование (можно легко подменить зависимости на фейковые).  

Dishka особенно удобно использовать с:  
- **FastAPI**  
- **aiohttp**  
- **Telegram-ботами**  

---

## 🎯 Зачем использовать DI?  
Без DI:  
```python
def get_user_service():
    db = create_db_session()
    kafka = KafkaProducer()
    return UserService(db, kafka)
```
➡️ Каждое место в коде само создаёт зависимости.  

С DI (Dishka):  
```python
user_service = container.get(UserService)
```
➡️ Dishka сама знает, как создать зависимости и передать их.  

---

## 🛠 Как внедрить Dishka в проект  

### 📦 Где хранить контейнер  
Создай файл `infrastructure/di.py` — он будет точкой входа для зависимостей:  

```python
from dishka import Provider, Scope
from infrastructure.db.mongo import get_mongo_client
from infrastructure.kafka.producer import get_kafka_producer
from infrastructure.db.session import get_db_session

# Контейнер зависимостей
provider = Provider(scope=Scope.APP)

# Регистрируем зависимости
provider.provide(get_mongo_client)
provider.provide(get_kafka_producer)
provider.provide(get_db_session)
```

---

### 🧠 Как регистрировать зависимости  
Функции, которые создают объекты, должны быть **асинхронными** (если работаешь с БД/сетевыми сервисами).  

Пример для SQLAlchemy:  
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

---

## 🧱 Архитектура проекта (Clean Architecture + DI)  

```bash
src/
├── api/             # Входные точки: роуты, схемы
│   ├── v1/
│   └── routers/
│   └── schemas/
│
├── application/     # Use-cases, бизнес-логика
│   ├── services/
│   └── interfaces/
│
├── domain/          # Сущности и value-объекты
│   ├── models/
│   └── schemas/
│
├── infrastructure/  # Реализация: БД, Kafka, Redis
│   ├── db/
│   ├── kafka/
│   ├── config.py
│   └── di.py        # DI контейнер
```

---

## 💡 Лучшие практики  

### 🔧 Инъекция в сервис  
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
➡️ Тут Dishka сама создаст `AsyncSession` и передаст в сервис.  

---

### 🚀 Интеграция с FastAPI  
```python
from fastapi import FastAPI
from dishka.integrations.fastapi import setup_dishka, DishkaRoute
from infrastructure.di import provider

app = FastAPI()

# Подключаем Dishka
setup_dishka(app, provider=provider, route_class=DishkaRoute)
```

---

### 🧪 Тестирование с подменой зависимостей  
```python
from dishka import Provider

# Тестовый контейнер
test_provider = Provider()

# Подменяем зависимости на фейки
test_provider.provide(lambda: MockKafkaProducer())
test_provider.provide(lambda: FakeMongoClient())

# Используем test_provider в тестовом FastAPI-приложении
```
➡️ Это удобно: можно тестировать код без настоящей БД или Kafka.  

---

## 📚 Полезные ссылки  
- 📘 Статья на Хабре: [«Год с Dishka»](https://habr.com/ru/articles/894286/)  
- 📦 GitHub Dishka: [github.com/reagento/dishka](https://github.com/reagento/dishka)  
