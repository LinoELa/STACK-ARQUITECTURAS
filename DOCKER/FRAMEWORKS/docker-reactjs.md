# Docker: ReactJS

Guia rapida de Docker para un frontend con ReactJS.

---

## 1. Estructura recomendada

```text
frontend-reference-reactjs/
|-- Dockerfile
|-- .dockerignore
|-- docker-compose.yml
|-- .env.example
|-- src/
|-- public/
`-- package.json
```

---

## 2. Base recomendada

- imagen base de build: `node:20-alpine`
- runtime frecuente: `nginx:alpine` si se sirven estaticos
- servicio principal: `web`
- servicio auxiliar frecuente: `api`
- comando de dev: `npm run dev`
- comando de arranque: servir `dist` o `build`

---

## 3. Compose tipico

Lo normal en ReactJS es levantar:

- `web`

Y si el frontend se prueba junto a la API:

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

- `DOCKER/BASE/dockerfile.md`
- `DOCKER/BASE/dockerignore.md`
- `FRONTEND/FRAMEWORKS/arquitectura-reactjs.md`
