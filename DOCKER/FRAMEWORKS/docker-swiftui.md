# Docker: SwiftUI

Guia rapida de Docker para un proyecto mobile con SwiftUI.

---

## 1. Regla importante

En SwiftUI, Docker no suele ejecutar el simulador ni la app iOS.

Docker suele servir para:

- API local
- mocks
- base de datos auxiliar
- tooling de equipo
- CI

---

## 2. Estructura recomendada

```text
mobile-reference-swiftui/
|-- Dockerfile.tools
|-- .dockerignore
|-- docker-compose.yml
|-- .env.example
|-- App/
|-- Tests/
`-- Package.swift o proyecto Xcode
```

---

## 3. Compose tipico

Lo normal aqui es levantar:

- `api`
- `db`
- `mock-server`

La app iOS sigue ejecutandose desde Xcode o el simulador local.

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
- `MOBILE/FRAMEWORKS/arquitectura-swiftui.md`
