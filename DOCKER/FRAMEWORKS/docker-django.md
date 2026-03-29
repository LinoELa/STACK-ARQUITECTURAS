# Docker: Django

Guia rapida de Docker para un backend con Django.

---

## 1. Estructura recomendada

```text
backend-reference-django/
|-- Dockerfile
|-- .dockerignore
|-- docker-compose.yml
|-- .env.example
|-- manage.py
|-- requirements.txt
|-- app/
`-- config/
```

---

## 2. Base recomendada

- imagen base: `python:3.12-slim`
- servicio principal: `app`
- servicio auxiliar frecuente: `db`
- comando de dev: `python manage.py runserver 0.0.0.0:8000`
- comando de arranque: `gunicorn config.wsgi:application --bind 0.0.0.0:8000`

---

## 3. Compose tipico

Lo normal en Django es levantar:

- `app`
- `db`

Y a veces tambien:

- `redis`
- `worker`

---

## 4. Comandos base

```bash
docker compose up --build
docker compose run --rm app python manage.py migrate
docker compose logs -f app
docker compose down
```

---

## 5. Referencia relacionada

Ver tambien:

- `DOCKER/BASE/dockerignore.md`
- `DOCKER/BASE/docker-compose.md`
- `BACKEND/FRAMEWORKS/arquitectura-django.md`
