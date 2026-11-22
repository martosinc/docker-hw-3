
# Отчёт

## 4 балла

```bash
docker build -t counter-app .
docker run -d --name counter -p 8080:8000 -e GREETING='Hello, Docker!' counter-app
```

Проверка:

```bash
curl localhost:8080/
curl -X POST localhost:8080/increment
curl -X POST localhost:8080/increment
curl localhost:8080/
```

Результат: `count: 2`.

Лог: `reports/level4.log`.

## 6 баллов

После 6 запросов `POST /increment`:

```bash
docker compose down
docker compose up -d
curl localhost:8080/
```

Результат:

```json
{"greeting": "Hello, Docker!", "count": 6, "hostname": "b1a8dcd11004", "storage": "sqlite"}
```

## 8 баллов

```bash
docker compose up -d --build
docker compose ps
```

Оба контейнера `healthy`.

```bash
curl -X POST localhost:8080/increment
curl -X POST localhost:8080/increment
docker compose down
docker compose up -d
curl localhost:8080/
```

Результат: `count: 2`, `storage: redis`.

Проверка отключения Redis:

```bash
docker compose stop redis

curl -s -o /dev/null -w '%{http_code}\n' localhost:8080/healthz
curl -s -o /dev/null -w '%{http_code}\n' localhost:8080/readyz
curl -s -o /dev/null -w '%{http_code}\n' -X POST localhost:8080/increment
```

Результат:

```text
200
503
503
```

Восстановление:

```bash
docker compose start redis
docker compose ps
curl localhost:8080/
curl -X POST localhost:8080/increment
```

`GET` снова возвращает `2`, а следующий `POST` `3`