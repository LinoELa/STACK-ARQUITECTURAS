# Docker: Kotlin

Guia rapida de Docker para un proyecto mobile con Kotlin.

---

## 1. Regla importante

Si Kotlin aqui significa Android nativo, Docker no suele ejecutar la app ni el emulador.

Docker suele servir para:

- tooling
- CI
- API local
- mocks
- base de datos auxiliar

---

## 2. Estructura recomendada

```text
mobile-reference-kotlin/
|-- Dockerfile.tools
|-- .dockerignore
|-- docker-compose.yml
|-- .env.example
|-- app/
|-- gradle/
`-- build.gradle.kts
```

---

## 3. Compose tipico

Lo normal aqui es levantar:

- `api`
- `db`
- `mock-server`

La compilacion y el emulador siguen principalmente fuera de Docker.

---

## 4. Comandos base

```bash
docker compose up -d api db
docker compose logs -f api
docker compose down
```

---

## 5. Referencia relacionada

Ver tambien:

- `DOCKER/BASE/estructura-base.md`
- `DOCKER/BASE/docker-compose.md`
- `MOBILE/FRAMEWORKS/arquitectura-kotlin.md`
