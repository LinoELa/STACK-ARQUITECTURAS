# Docker: NestJS

Guia rapida de Docker para un backend con NestJS.

---

## 1. Estructura recomendada

```text
backend-reference-nestjs/
|-- Dockerfile
|-- .dockerignore
|-- docker-compose.yml
|-- .env.example
|-- src/
|-- prisma/ o database/
`-- package.json
```

---

## 2. Base recomendada

- imagen base: `node:20-alpine`
- servicio principal: `app`
- servicios auxiliares frecuentes: `db`, `redis`
- comando de dev: `npm run start:dev`
- comando de arranque: `npm run start:prod`

---

## 3. Compose tipico

Lo normal en NestJS es levantar:

- `app`
- `db`
- `redis` si hay cache, colas o sesiones

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

- `DOCKER/BASE/dockerfile.md`
- `DOCKER/BASE/docker-commands.md`
- `BACKEND/FRAMEWORKS/arquitectura-nestjs.md`
