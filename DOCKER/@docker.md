# Carpeta DOCKER

Este archivo explica que representa la carpeta `DOCKER`, para que sirve y como se lee.

---

## 1. Que es esta carpeta

La carpeta `DOCKER` guarda referencias para containerizar proyectos del repositorio.

Aqui la idea principal no es explicar negocio ni arquitectura interna.
Aqui la idea principal es dejar una base comun para levantar proyectos con:

- `Dockerfile`
- `.dockerignore`
- `docker-compose.yml`
- comandos con `docker compose`

---

## 2. Para que sirve

Esta carpeta sirve para:

- definir una estructura Docker comun para cualquier proyecto
- evitar que cada repo invente su propio `Dockerfile`
- dejar un `docker-compose.yml` por proyecto
- documentar como levantar app, base de datos y servicios auxiliares
- adaptar esa base segun el framework

---

## 3. Que contiene

Esta carpeta contiene:

- `@docker.md`
- `BASE/`
- `FRAMEWORKS/`

### `BASE/`

Guarda la plantilla comun que deberia repetirse en casi cualquier proyecto:

- `estructura-base.md`
- `dockerfile.md`
- `docker-compose.md`
- `dockerignore.md`
- `docker-commands.md`

### `FRAMEWORKS/`

Guarda ejemplos por framework:

- ExpressJS
- NestJS
- Django
- Flask
- Spring Boot
- Next.js
- ReactJS
- React Native
- Flutter
- Kotlin
- SwiftUI

---

## 4. Regla principal

La regla recomendada es esta:

- cada proyecto tiene su propio `Dockerfile`
- cada proyecto tiene su propio `.dockerignore`
- cada proyecto tiene su propio `docker-compose.yml`

Si el proyecto crece, entonces puede agregar:

- `docker-compose.dev.yml`
- `docker-compose.prod.yml`
- una carpeta `docker/` para scripts o configuraciones auxiliares

---

## 5. Como leer esta carpeta

La lectura recomendada es esta:

1. entrar en `BASE/`
2. leer `estructura-base.md`
3. leer `dockerfile.md`, `docker-compose.md` y `dockerignore.md`
4. mirar `docker-commands.md`
5. despues abrir el archivo del framework que corresponda

---

## 6. Idea de uso

La idea no es copiar todo a ciegas.

La idea es usar esta carpeta como plantilla para decidir:

- que archivos Docker necesita el proyecto
- como nombrar los servicios del compose
- que comandos usar para levantar el entorno
- cuando hace falta una version simple o una version mas completa

---

## 7. Resumen rapido

La carpeta `DOCKER` sirve para dejar una base reutilizable de Docker en todo el repositorio.

Su idea principal es:

- una estructura minima comun
- un `compose` por proyecto
- comandos claros con `docker compose`
- ejemplos adaptados por framework
