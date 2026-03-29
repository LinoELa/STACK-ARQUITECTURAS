# Estructura Base Docker

Este archivo explica cual es la estructura minima que deberia tener un proyecto cuando se documenta o se prepara para Docker.

---

## 1. Regla principal

Para casi cualquier proyecto, la base minima recomendada es esta:

- `Dockerfile`
- `.dockerignore`
- `docker-compose.yml`

Si necesitas separar entornos, puedes agregar:

- `docker-compose.dev.yml`
- `docker-compose.prod.yml`
- `docker/`

---

## 2. Estructura minima

```text
mi-proyecto/
|-- Dockerfile
|-- .dockerignore
|-- docker-compose.yml
|-- .env.example
|-- src/
|-- package.json / requirements.txt / pom.xml / pubspec.yaml
`-- README.md
```

Esta estructura sirve bien para:

- backends pequenos o medianos
- frontends web
- APIs con una sola base de datos
- proyectos que todavia no necesitan separar dev y prod

---

## 3. Estructura ampliada

```text
mi-proyecto/
|-- Dockerfile
|-- .dockerignore
|-- docker-compose.yml
|-- docker-compose.dev.yml
|-- docker-compose.prod.yml
|-- .env.example
|-- docker/
|   |-- nginx/
|   |-- scripts/
|   `-- volumes/
|-- src/
`-- README.md
```

Esta estructura tiene sentido cuando:

- el proyecto usa varios servicios
- necesitas separar dev y prod
- hay `nginx`, workers o scripts de arranque
- quieres ordenar mejor la parte de infraestructura

---

## 4. Donde vive cada archivo

La recomendacion base es esta:

- `Dockerfile` en la raiz del proyecto
- `.dockerignore` en la raiz del proyecto
- `docker-compose.yml` en la raiz del proyecto
- `docker/` solo para archivos auxiliares

No metas el `Dockerfile` escondido dentro de `src/`.
No mezcles un `compose` de un proyecto con otro proyecto distinto.

---

## 5. Un compose por proyecto

La regla sana suele ser esta:

- un proyecto backend -> su `docker-compose.yml`
- un proyecto frontend -> su `docker-compose.yml`
- un proyecto mobile -> su `docker-compose.yml` si usa servicios auxiliares o tooling

Excepcion comun:

- un monorepo que quiera levantar todo junto desde una raiz comun

En ese caso puedes tener:

- `apps/api/docker-compose.yml`
- `apps/web/docker-compose.yml`
- y otro `docker-compose.yml` en la raiz para orquestar todo

---

## 6. Nombres tipicos de servicios

Usa nombres simples y repetibles:

- `app`
- `web`
- `api`
- `db`
- `redis`
- `worker`
- `nginx`
- `mock-server`

Eso hace que los comandos sean faciles de recordar:

```bash
docker compose up --build
docker compose exec app sh
docker compose logs -f db
```

---

## 7. Resumen rapido

La estructura Docker minima para cualquier proyecto suele ser:

- `Dockerfile`
- `.dockerignore`
- `docker-compose.yml`

Y si el proyecto crece:

- `docker-compose.dev.yml`
- `docker-compose.prod.yml`
- `docker/`
