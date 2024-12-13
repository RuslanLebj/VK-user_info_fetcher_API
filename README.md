# VK user graph API
VK user graph API — это API для работы с графовой базой данных Neo4j, предназначенное для управления графом данных, таких как пользователи, фолловеры и подписки. API позволяет получать данные, добавлять новые узлы и связи, а также удалять их.

## Функционал
- **Чтение данных из Neo4j:**
  - Получение всех узлов с их метками и ID.
  - Получение узла, всех его связей и связанных узлов.
- **Добавление данных:**
  - Создание новых узлов и связей в графе.
- **Удаление данных:**
  - Удаление узлов и связанных с ними связей.

## Установка и запуск

### Локальный запуск

#### Требования
- `Python`: версия 3.12.3
- `Poetry`: версия 1.8.3

#### Шаги установки
1. **Клонируйте репозиторий, зайдите в каталог проекта:**

```bash
git clone https://github.com/RuslanLebj/VK-user-graph-API
cd vk_user_graph_api
```

2. **Установите зависимости:**

```bash
poetry install
```

3. **Настройте переменные окружения: скопируйте данные из файла `template.env` в `.env`, 
измените данные при необходимости (при локальном запуске надо раскомментировать `NEO4J_BOLT_URL=bolt://localhost:7687`, 
тк по умолчанию стоят параметры для запуска из контейнеров, где хост является названием контейнера с базой):**


4. **Запустите сервер:**

```bash
uvicorn src.app.main:app --reload --host 0.0.0.0 --port 8000
```

Сервис будет доступен по адресу: `http://0.0.0.0:8000`.

### Запуск через Docker контейнеры

#### Требования
- `Docker`
- `Docker-compose`
- `Make`

#### Шаги установки
1. **Клонируйте репозиторий, зайдите в каталог проекта:**

```bash
git clone https://github.com/RuslanLebj/VK-user-graph-API
cd vk_user_graph_api
```

2. **Настройте переменные окружения: скопируйте данные из файла `template.env` в `.env`, измените данные при необходимости:**


3. **Для запуска контейнеров выполните:**
```bash
make up
```

## Описание точек доступа API
1. **Получение всех узлов**
- **Endpoint:** `/api/nodes`
- **Метод:** `GET`
- **Описание:** Возвращает список всех узлов с их ID и метками.
- **Пример ответа:**
```json
[
    {"id": 1, "label": "User"},
    {"id": 2, "label": "Group"}
]
```
2. **Получение узла и его связей**
- **Endpoint:** `/api/node/{node_id}`
- **Метод:** `GET`
- **Описание:** Возвращает узел, его связи и связанные узлы.
- **Пример ответа:**
```json
[
    {"n": {"id": 1, "name": "John Doe"}, "r": {"type": "Follow"}, "m": {"id": 2, "name": "Jane Doe"}}
]
```
3. **Добавление узла и связей**
- **Endpoint:** `/api/node/{node_id}`
- **Метод:** `POST`
- **Авторизация:** Требуется токен в заголовке `Authorization`.
- **Описание:** Создает узел и добавляет связи.
- **Тело запроса:**
```json
{
    "node": {
        "id": 3,
        "label": "User",
        "name": "Jane Doe",
        "screen_name": "janedoe",
        "sex": 2,
        "home_town": "San Francisco"
    },
    "relationships": [
        {"id": 20, "type": "Follow", "end_node_id": 1}
    ]
}
```
- **Пример ответа:**
```json
{"status": "Node and relationships added"}
```
4. **Удаление узла и связей**
- **Endpoint:** `/api/node/{node_id}`
- **Метод:** `DELETE`
- **Авторизация:** Требуется токен в заголовке `Authorization`.
- **Описание:** Удаляет узел и все его связи.
- **Пример ответа:**
```json
{"status": "Node 3 and its relationships deleted"}
```
## Тестирование (локально)
**Запуск тестов:**
1. Перенести `.env` в каталог `tests`
2. Убедитесь что база не содержит данных
3. Для тестирования используется `pytest`. Запустите тесты командой:

```bash
pytest
```
**Тестируемые сценарии:**
1. Проверка добавления узла и связей
2. Проверка получения всех узлов.
3. Проверка получения узла и его связей. .
4. Проверка удаления узла и связей.
