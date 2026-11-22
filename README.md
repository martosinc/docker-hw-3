## Запуск

```bash
cp .env.example .env
docker compose up -d --build
```

Проверка:

```bash
docker compose ps
curl localhost:8080/
curl -X POST localhost:8080/increment
```

Остановка:

```bash
docker compose down
```

## Настройки Compose

`.env`:
- `HOST_PORT` — порт на компьютере (:8080)
- `GREETING` — по умолчанию `Hello, Docker!`

`compose.yaml`:
- `app` — python приложение, порт внутри контейнера 8000, `STORAGE=redis`.
- `redis` — Redis 7.4, данные сохраняются в volume `redis_data`.

Удаление volume:

```bash
docker compose down -v
```