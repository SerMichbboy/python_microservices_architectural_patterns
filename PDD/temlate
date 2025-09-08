service/
  app/
    api/                # Входной слой (FastAPI/HTTP/RPC)
      v1/
        handlers.py
        routers.py
        schemas.py      # DTO для HTTP (Pydantic)
      deps.py           # DI провайдеры для маршрутов
      errors.py         # Маппинг исключений -> HTTP
      middleware.py
    application/        # Юзкейсы, транзакции
      use_cases/
        create_order.py
        get_order.py
      dto.py            # DTO для application-слоя
      interfaces.py     # Порты (абстракции репозиториев/шлюзов)
    domain/             # Чистый домен (без зависимостей)
      models.py
      value_objects.py
      services.py       # Доменные инварианты/правила
      exceptions.py
    infrastructure/     # Адаптеры к миру
      db/
        repositories.py
        models.py       # ORM (SQLAlchemy)
        migrations/     # Alembic
      messaging/
        publisher.py
        consumer.py
      cache/
        redis_cache.py
      clients/
        billing_client.py
      config.py         # Settings (pydantic-settings)
      logging.py
      observability.py  # Metrics/tracing/logging init
      unit_of_work.py
    main.py             # Точка входа сервиса
  tests/
    unit/
    integration/
    contract/
  deploy/
    Dockerfile
    docker-compose.yml  # для локалки
    k8s/                # манифесты/helm
  pyproject.toml
  Makefile
  README.md
