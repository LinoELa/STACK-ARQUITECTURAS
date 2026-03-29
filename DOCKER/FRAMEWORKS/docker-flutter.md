# Docker: Flutter

Guia rapida de Docker para un proyecto mobile con Flutter.

---

## 1. Regla importante

En Flutter, Docker suele usarse para:

- tooling
- CI
- mocks
- API local
- servicios auxiliares

No suele usarse para correr el simulador dentro del contenedor.

---

## 2. Estructura recomendada

```text
mobile-reference-flutter/
|-- Dockerfile.tools
|-- .dockerignore
|-- docker-compose.yml
|-- .env.example
|-- lib/
|-- android/
|-- ios/
`-- pubspec.yaml
```

---

## 3. Compose tipico

Lo normal aqui es levantar:

- `api`
- `db`
- `mock-server`

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

- `DOCKER/BASE/dockerignore.md`
- `DOCKER/BASE/docker-commands.md`
- `MOBILE/FRAMEWORKS/arquitectura-flutter.md`
