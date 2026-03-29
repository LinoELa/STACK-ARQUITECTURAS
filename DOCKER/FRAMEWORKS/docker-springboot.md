# Docker: Spring Boot

Guia rapida de Docker para un backend con Spring Boot.

---

## 1. Estructura recomendada

```text
backend-reference-springboot/
|-- Dockerfile
|-- .dockerignore
|-- docker-compose.yml
|-- .env.example
|-- pom.xml o build.gradle
`-- src/
```

---

## 2. Base recomendada

- build frecuente: Maven o Gradle
- imagen de runtime habitual: `eclipse-temurin:21-jre`
- servicio principal: `app`
- servicio auxiliar frecuente: `db`
- comando de arranque: `java -jar app.jar`

---

## 3. Compose tipico

Lo normal en Spring Boot es levantar:

- `app`
- `db`

Y a veces tambien:

- `redis`
- `rabbitmq`

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
- `DOCKER/BASE/docker-compose.md`
- `BACKEND/FRAMEWORKS/arquitectura-springboot.md`
