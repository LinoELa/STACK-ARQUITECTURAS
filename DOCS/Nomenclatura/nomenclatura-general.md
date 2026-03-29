# Nomenclatura General

## Objetivo

Este documento define una plantilla general de nomenclatura para todo el repositorio.

La idea es usar un sistema claro, consistente y reutilizable, evitando:

- mezcla de idiomas
- mezcla de estilos de nombres
- nombres duplicados para la misma idea
- excepciones improvisadas segun el proyecto

Este archivo ya no define solo un caso concreto.
Ahora funciona como template base para cualquier proyecto del repo.

---

## 1. Sistema 

### Regla corta

La regla por defecto es esta:

- `camelCase` dentro del codigo
- `PascalCase` para tipos y componentes
- `kebab-case` para archivos y carpetas
- `UPPER_SNAKE_CASE` para constantes globales
- `snake_case` solo cuando una integracion externa lo exija

Si no hay una razon fuerte para romper esta regla, no se rompe.


### Sistema de nomenclatura base

Este es el sistema recomendado y por defecto.

| Caso                        | Formato            | Uso recomendado                                                  | Ejemplo                                                 |
| --------------------------- | ------------------ | ---------------------------------------------------------------- | ------------------------------------------------------- |
| Variables y funciones       | `camelCase`        | valores, funciones, helpers, hooks, servicios internos           | `buildJsonDiff`, `requestBody`, `useLoginPage`          |
| Clases, tipos y componentes | `PascalCase`       | clases, errores, entidades, React components, DTOs tipados       | `AppError`, `LoginPage`, `UserSession`                  |
| Archivos y carpetas         | `kebab-case`       | nombres de archivos y directorios tecnicos                       | `json-processing`, `login-page.tsx`, `error-handler.ts` |
| Constantes globales         | `UPPER_SNAKE_CASE` | env vars, flags estaticos, constantes globales                   | `API_BASE_URL`, `DEFAULT_TIMEOUT_MS`                    |
| Casos especiales externos   | `snake_case`       | solo si la BD, API externa o contrato externo ya obliga a usarlo | `created_at`, `user_id`                                 |

---

## 2. Regla principal del repo

En este repositorio:

- el codigo se nombra en ingles
- la estructura tecnica se nombra en ingles
- la documentacion y explicaciones pueden escribirse en espanol

Eso significa:

- nombres tecnicos en ingles
- comentarios del equipo en espanol si hace falta
- Markdown de explicacion en espanol o bilingue si conviene

---

## 3. Que debe ir en ingles

Estas piezas deben usar nombres en ingles:

- carpetas tecnicas
- nombres de modulos
- nombres de slices
- nombres de acciones
- nombres de vistas, screens o pages
- archivos de codigo
- endpoints HTTP
- funciones, variables y constantes
- clases, DTOs, services, controllers, hooks y middlewares

Ejemplos correctos:

- `json-processing`
- `compare-post`
- `workspace.module.ts`
- `login-page.tsx`
- `validate-json.service.ts`
- `requestLogger`
- `POST /json/validate`

---

## 4. Que puede ir en espanol

Estas piezas pueden estar en espanol:

- documentacion del equipo en `DOCS/`
- explicaciones dentro de archivos `@...md`
- comentarios dentro del codigo si el equipo lo prefiere
- notas de coordinacion

Lo importante es no mezclar idiomas dentro de una misma pieza tecnica.

Por ejemplo:

- correcto: codigo en ingles y comentario en espanol
- incorrecto: `procesarJsonService`, `json-analisis`, `validateDocumento`

---

## 5. Regla para carpetas

### Carpetas de codigo

Usan ingles y `kebab-case`.

Ejemplos:

- `core`
- `json-processing`
- `shared`
- `validate`
- `compare-post`
- `user-settings`

### Carpetas de documentacion

En documentacion se pueden usar nombres de alto nivel mas legibles, pero conviene mantener criterio.

Ejemplos:

- `Architecture`
- `Reference`
- `Nomenclatura`
- `Hexagonal`
- `Modular`

Si la carpeta es tecnica de codigo, mejor `kebab-case`.
Si la carpeta es documental de alto nivel, puede mantenerse estilo titulo si aporta claridad.

---

## 6. Regla para archivos

### Archivos tecnicos generales

Usan:

```text
feature-name.suffix.ext
```

Ejemplos:

- `login.service.ts`
- `error-handler.middleware.ts`
- `api-client.ts`

### Modulos

Usan:

```text
module-name.module.ts
```

Ejemplos:

- `core.module.ts`
- `workspace.module.ts`
- `compare.module.ts`

### Controllers

Usan:

```text
action-name.controller.ts
```

Ejemplos:

- `about.controller.ts`
- `validate-json.controller.ts`

### Services

Usan:

```text
action-name.service.ts
```

Ejemplos:

- `compare-json.service.ts`
- `login.service.ts`

### DTOs o schemas

Usan:

```text
action-name.dto.ts
```

o

```text
action-name.schema.ts
```

Ejemplos:

- `validate-json.dto.ts`
- `login.schema.ts`

### Hooks

Usan:

```text
use-feature-name.ts
```

Ejemplos:

- `use-login-page.ts`
- `use-workspace-screen.ts`

### Components

Usan archivo en `kebab-case`, aunque el export interno pueda ser `PascalCase`.

Ejemplos:

- `login-form.tsx`
- `diff-viewer.tsx`
- `screen-shell.tsx`

### Middlewares

Usan:

```text
middleware-name.middleware.ts
```

Ejemplos:

- `request-logger.middleware.ts`
- `error-handler.middleware.ts`

### Utils

Usan:

```text
utility-name.util.ts
```

o si el equipo prefiere menos sufijos:

```text
utility-name.ts
```

La regla importante aqui es no mezclar ambos estilos sin criterio.

---

## 7. Regla para funciones, variables y constantes

### Variables y funciones

Usan ingles tecnico y `camelCase`.

Ejemplos:

- `validateJson`
- `buildJsonDiff`
- `parsedJson`
- `requestBody`

### Clases, entidades, tipos y componentes

Usan `PascalCase`.

Ejemplos:

- `AppError`
- `User`
- `LoginPage`
- `JsonDocument`

### Constantes globales

Usan `UPPER_SNAKE_CASE`.

Ejemplos:

- `API_BASE_URL`
- `MAX_RETRY_COUNT`
- `DEFAULT_LANGUAGE`

### `snake_case`

No se usa como estilo general del codigo.

Solo se acepta cuando:

- la base de datos ya viene asi
- una API externa ya viene asi
- un contrato externo no se puede cambiar

Ejemplos:

- `created_at`
- `user_id`
- `updated_by`

En ese caso, conviene mapearlo a la convención interna del proyecto cuando sea posible.

---

## 8. Regla para endpoints

Los endpoints tambien van en ingles.

Ejemplos:

- `GET /about`
- `GET /health`
- `POST /json/validate`
- `POST /json/format`
- `POST /json/analyze`
- `POST /json/compare`

Si el endpoint representa un recurso o una accion, el nombre debe ser:

- simple
- predecible
- consistente con el resto de endpoints

---

## 9. Regla para comentarios

Los comentarios del codigo pueden ir en espanol si ayudan al equipo.

Deben explicar:

- la intencion
- la decision tecnica
- el paso importante del flujo

No deben explicar cosas obvias.

Ejemplo util:

```ts
// Intentamos parsear el JSON una sola vez para reutilizar el resultado
// en validacion, analisis y formateo.
```

Ejemplo flojo:

```ts
// Guardamos el valor en una variable.
```

---

## 10. Regla de consistencia

Una misma idea debe tener un solo nombre en todo el proyecto.

Ejemplo:

- si elegimos `json-processing`, no mezclar despues `jsonProcessor`, `procesador-json` o `json-tools` para la misma capa
- si elegimos `validate-json.service.ts`, no crear luego `json-validator.service.ts` para la misma responsabilidad
- si elegimos `workspace`, no mezclar despues `editor-space` para representar el mismo bloque

---

## 11. Checklist rapido antes de crear nombres nuevos

Antes de crear una carpeta o archivo nuevo, revisa:

1. si el nombre puede quedar en ingles simple
2. si sigue la convención principal del repo
3. si realmente necesita `snake_case` o no
4. si ya existe otra pieza con el mismo significado pero otro nombre
5. si el nombre ayuda a entender rapido la responsabilidad

---

## 12. Regla final del template

La regla base de este template es esta:

- ingles para estructura y codigo
- espanol para explicar a las personas
- `camelCase` dentro del codigo
- `PascalCase` para tipos y componentes
- `kebab-case` para archivos y carpetas
- `UPPER_SNAKE_CASE` para constantes globales
- `snake_case` solo cuando un sistema externo lo obligue

Si todo el repo sigue esta base, la nomenclatura deja de discutirse y pasa a ser una convencion estable.
