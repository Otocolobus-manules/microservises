# Microservices - Blog API

Асинхронный REST API для управления блогом на FastAPI с поддержкой пользователей, статей и комментариев, а также JWT-аутентификации.

---

## Функциональность

* **Пользователи**

  * Регистрация и авторизация
  * Обновление профиля
* **Статьи**

  * Создание, чтение, обновление и удаление статей
* **Комментарии**

  * Добавление и удаление комментариев к статьям
* **Аутентификация**

  * JWT-токены для защищённых эндпоинтов
* **Асинхронность**

  * Полная поддержка `async/await`
* **CORS**

  * Разрешены кросс-доменные запросы

---

## Требования

* Docker и Docker Compose

---

## Установка и запуск

### 1. Клонирование репозитория

```bash
git clone <repository-url>
cd microservices
```

### 2. Запуск через Docker Compose

```bash
docker-compose up -d
```

Это создаст и запустит:

* Контейнер с PostgreSQL
* Приложение FastAPI

API будет доступно по адресу: [http://localhost:8000](http://localhost:8000)

### 3. Остановка приложения

```bash
docker-compose down
```

---

## Документация API

### Swagger UI

[http://localhost:8000/api/docs](http://localhost:8000/api/docs)


---

## Основные эндпоинты

### Здоровье приложения

* **GET /health** - проверка статуса приложения

```json
{"status": "ok"}
```

### Пользователи

* **POST /api/users/** - регистрация нового пользователя

```json
{
  "email": "user@example.com",
  "username": "username",
  "password": "password123"
}
```

* **POST /api/users/login** - логин и получение JWT токена

```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

Ответ:

```json
{
  "access_token": "<JWT_TOKEN>",
  "token_type": "bearer"
}
```

* **GET /api/users/me** - получение текущего пользователя (требуется JWT)
* **PUT /api/users/me** - обновление профиля текущего пользователя

### Статьи

* **POST /api/articles/** - создание статьи (требуется JWT)

```json
{
  "title": "Заголовок статьи",
  "description": "Описание",
  "body": "Содержание статьи"
}
```

* **GET /api/articles/?skip=0&limit=20** - получение всех статей
* **GET /api/articles/{slug}** - получение статьи по slug
* **PUT /api/articles/{slug}** - обновление статьи (только автор)
* **DELETE /api/articles/{slug}** - удаление статьи (только автор)

### Комментарии

* **POST /api/articles/{slug}/comments** - добавить комментарий (требуется JWT)

```json
{
  "body": "Текст комментария"
}
```

* **GET /api/articles/{slug}/comments?skip=0&limit=20** - получение комментариев статьи
* **DELETE /api/articles/{slug}/comments/{comment_id}** - удалить комментарий (только автор)

---

## Структура проекта

```
microservices/
├── src/
│   ├── main.py                 # Точка входа приложения
│   ├── controllers/            # Бизнес-логика
│   │   ├── article.py
│   │   ├── comment.py
│   │   └── user.py
│   ├── routes/                 # API маршруты
│   │   ├── articles.py
│   │   ├── comments.py
│   │   └── users.py
│   ├── models/                 # SQLAlchemy модели
│   │   ├── article.py
│   │   ├── comment.py
│   │   └── user.py
│   ├── schemas/                # Pydantic схемы
│   │   ├── article.py
│   │   ├── comment.py
│   │   └── user.py
│   ├── repositories/           # Слой доступа к данным
│   │   ├── base.py
│   │   ├── article.py
│   │   ├── comment.py
│   │   └── user.py
│   ├── core/                   # Конфигурация и утилиты
│   │   ├── config.py
│   │   ├── database.py
│   │   └── security.py
│   └── dependencies.py         # Зависимости FastAPI
├── alembic/                    # Миграции БД
├── Dockerfile                  # Docker конфигурация
├── docker-compose.yml          # Docker Compose
├── pyproject.toml              # Poetry конфигурация
└── README.md                   # Документация проекта
```

---

## Архитектура

* **Routes** — эндпоинты и валидация запросов
* **Controllers** — бизнес-логика приложения
* **Repositories** — абстракция доступа к данным
* **Models** — ORM модели SQLAlchemy
* **Schemas** — Pydantic модели для валидации

---

## CI/CD (GitHub Actions + Render)

* Сборка Docker-образа и публикация в GitHub Container Registry (GHCR)
* Автоматический деплой на Render
* Настройка переменных окружения и healthcheck для сервиса
* Автодеплой при изменениях в ветке `main`

---

## Контакты

Для вопросов и предложений используйте Issues в репозитории.
