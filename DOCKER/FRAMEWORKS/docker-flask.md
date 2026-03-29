# Docker: Flask

Guia rapida de Docker para un backend con Flask.

---

## 1. Estructura recomendada

```text
backend-reference-flask/
|-- Dockerfile
|-- .dockerignore
|-- docker-compose.yml
|-- .env.example
|-- requirements.txt
|-- app/
`-- wsgi.py
```

---

## 2. Base recomendada

- imagen base: `python:3.12-slim`
- servicio principal: `app`
- servicio auxiliar frecuente: `db`
- comando de dev: `flask run --host=0.0.0.0 --port=5000`
- comando de arranque: `gunicorn wsgi:app --bind 0.0.0.0:5000`

---

## 3. Compose tipico

Lo normal en Flask es levantar:

- `app`
- `db`

---

## 4. Comandos base

```bash
docker compose up --build
docker compose logs -f app
docker compose exec app sh
docker compose down
```

---

## 5. Referencia relacionada

Ver tambien:

- `DOCKER/BASE/estructura-base.md`
- `DOCKER/BASE/docker-commands.md`
- `BACKEND/FRAMEWORKS/arquitectura-flask.md`
