# Carpeta Hexagonal

Este archivo explica que representa esta carpeta `Hexagonal`, para que sirve y como se lee.

---

## 1. Que es esta carpeta

La carpeta `Hexagonal` guarda referencias de arquitectura backend basadas en:

- Hexagonal
- Vertical Slicing
- Screaming Architecture

Aqui la idea principal no es organizar el proyecto primero por carpetas globales como:

- `controllers`
- `routes`
- `services`
- `repositories`

Aqui la idea principal es organizar el backend por slices del negocio y por capas claras.

En el ejemplo de referencia, esos slices son:

- `auth`
- `movies`
- `watchlist`

---

## 2. Para que sirve

Esta carpeta sirve para:

- documentar como se organiza un backend hexagonal
- mostrar una version MVP o minimalista
- mostrar una version completa
- dejar una guia visual con archivos y carpetas reales

No es codigo ejecutable.
Es una referencia para disenar y entender la estructura de un proyecto.

---

## 3. Que archivos tiene

Esta carpeta contiene:

- `@hexagonal.md`
- `Arquitectura-Hexagonal-MVP.md`
- `Arquitectura-Hexagonal-Completo.md`

### `@hexagonal.md`

Explica la propia carpeta `Hexagonal`.

### `Arquitectura-Hexagonal-MVP.md`

Muestra la version base o minimalista de la arquitectura hexagonal aplicada a un backend de referencia.

### `Arquitectura-Hexagonal-Completo.md`

Muestra una version mas profunda y preparada para crecimiento.

---

## 4. Como se lee esta arquitectura

La lectura correcta es esta:

- primero se mira el slice del negocio
- despues se mira la capa interna del slice
- al final se mira la accion o adaptador concreto

Ejemplo mental:

```text
auth/
|-- domain/
|   |-- entities/
|   `-- ports/
|-- application/
|   |-- login/
|   `-- register/
`-- infrastructure/
    |-- persistence/
    |-- security/
    `-- http/
```

Eso significa:

- `auth` es el slice funcional
- `domain` contiene negocio y contratos
- `application` contiene casos de uso
- `infrastructure` contiene detalles tecnicos

---

## 5. Regla de estructura en Hexagonal

En esta carpeta, la idea de base es esta:

- cada feature vive junta en su propio slice
- cada slice se separa en `domain`, `application` e `infrastructure`
- el dominio no conoce tecnologia concreta
- la aplicacion usa contratos del dominio
- la infraestructura implementa esos contratos

La direccion de dependencias es:

```text
infrastructure -> application -> domain
```

---

## 6. Diferencia entre MVP y Completo

### MVP

La version MVP:

- usa menos niveles internos
- mantiene la estructura base del slice
- es suficiente para empezar un backend limpio sin sobrecargarlo

### Completo

La version completa:

- separa mejor controllers, persistence, validation y security
- organiza mejor los casos de uso por accion
- esta pensada para crecimiento real

---

## 7. Cuando usar esta carpeta

Usa esta carpeta cuando quieras:

- explicar arquitectura hexagonal a otra persona
- comparar una version base contra una version mas avanzada
- tomar esta estructura como plantilla para un backend nuevo
- entender como combinar Hexagonal, Vertical Slicing y Screaming

---

## 8. Resumen rapido

La carpeta `Hexagonal` sirve para explicar una arquitectura backend donde:

- el negocio se organiza por slices como `auth`, `movies` y `watchlist`
- cada slice tiene `domain`, `application` e `infrastructure`
- el dominio no depende de Express, Prisma ni JWT
- la tecnologia queda en los bordes
- el repositorio grita negocio antes que tecnologia
