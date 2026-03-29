# Docker: React Native

Guia rapida de Docker para un proyecto mobile con React Native.

---

## 1. Regla importante

En React Native, Docker no suele ejecutar el emulador ni la app final.

Docker suele servir para:

- tooling
- CI
- API local
- mocks
- base de datos auxiliar

---

## 2. Estructura recomendada

```text
mobile-reference-react-native/
|-- Dockerfile.tools
|-- .dockerignore
|-- docker-compose.yml
|-- .env.example
|-- src/
|-- android/
|-- ios/
`-- package.json
```

---

## 3. Compose tipico

Lo normal aqui es levantar servicios de apoyo:

- `api`
- `db`
- `mock-server`

El emulador y la app nativa siguen fuera de Docker.

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
- `DOCKER/BASE/docker-commands.md`
- `MOBILE/FRAMEWORKS/arquitectura-react-native.md`
