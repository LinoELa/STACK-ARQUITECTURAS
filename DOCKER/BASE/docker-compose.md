# Docker Compose

Este archivo explica como usar `docker-compose.yml` como punto de entrada del proyecto.

---

## 1. Para que sirve

`docker-compose.yml` sirve para levantar varios servicios juntos.

Por ejemplo:

- la app
- la base de datos
- redis
- workers
- mocks

La idea es que el proyecto pueda arrancar con un comando facil:

```bash
docker compose up --build
```

---

## 2. Forma minima

```yaml
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "5600:5600"
    env_file:
      - .env
    volumes:
      - .:/app
    depends_on:
      - db

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: app
      POSTGRES_USER: app
      POSTGRES_PASSWORD: app
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

---

## 3. Piezas principales

Las piezas que mas se repiten son estas:

- `services`
- `build`
- `ports`
- `env_file`
- `environment`
- `volumes`
- `depends_on`

---

## 4. Nombres recomendados

Usa nombres cortos:

- `app`
- `web`
- `api`
- `db`
- `redis`
- `worker`

Eso deja comandos faciles:

```bash
docker compose exec app sh
docker compose logs -f db
docker compose restart web
```

---

## 5. Dev y prod

Puedes empezar con un solo archivo:

- `docker-compose.yml`

Y separar despues cuando haga falta:

- `docker-compose.dev.yml`
- `docker-compose.prod.yml`

Ejemplo de uso:

```bash
docker compose -f docker-compose.yml -f docker-compose.dev.yml up --build
```

---

## 6. Regla practica

Si el proyecto es pequeno:

- un `compose` simple

Si el proyecto tiene base de datos, cache y varios procesos:

- un `compose` principal y variantes por entorno

---

## 7. Resumen rapido

`docker-compose.yml` es la puerta de entrada para levantar el entorno del proyecto.

Su trabajo es:

- construir la app
- conectar servicios
- exponer puertos
- montar volumenes
- hacer que `docker compose up` sea el comando normal de arranque
