# Docker: Next.js

Guia rapida de Docker para un frontend con Next.js.

---

## 1. Estructura recomendada

```text
frontend-reference-nextjs/
|-- Dockerfile
|-- .dockerignore
|-- docker-compose.yml
|-- .env.example
|-- src/ o app/
|-- public/
`-- package.json
```

---

## 2. Base recomendada

- imagen base: `node:20-alpine`
- servicio principal: `web`
- servicio auxiliar frecuente: `api` si el frontend se levanta junto a su backend
- comando de dev: `npm run dev`
- comando de arranque: `npm run build && npm run start`

---

## 3. Compose tipico

Lo normal en Next.js es levantar:

- `web`

Y si el proyecto quiere todo junto:

- `web`
- `api`

---

## 4. Comandos base

```bash
docker compose up --build
docker compose logs -f web
docker compose exec web sh
docker compose down
```

---

## 5. Referencia relacionada

Ver tambien:

- `DOCKER/BASE/docker-compose.md`
- `DOCKER/BASE/docker-commands.md`
- `FRONTEND/FRAMEWORKS/arquitectura-nextjs.md`
