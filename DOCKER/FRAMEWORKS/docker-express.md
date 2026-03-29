# Docker: ExpressJS

Guia rapida de Docker para un backend con ExpressJS.

---

## 1. Estructura recomendada

```text
backend-reference-express/
|-- Dockerfile
|-- .dockerignore
|-- docker-compose.yml
|-- .env.example
|-- src/
|-- prisma/
`-- package.json
```

---

## 2. Base recomendada

- imagen base: `node:20-alpine`
- servicio principal: `app`
- servicios auxiliares frecuentes: `db`, `redis`
- comando de dev: `npm run dev`
- comando de arranque: `npm run start`

---

## 3. Compose tipico

Lo normal en Express es levantar:

- `app`
- `db`
- `redis` si el proyecto usa cache o sesiones

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
- `DOCKER/BASE/docker-compose.md`
- `BACKEND/FRAMEWORKS/arquitectura-expressjs.md`
