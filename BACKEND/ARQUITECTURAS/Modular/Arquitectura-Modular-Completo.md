# Arquitectura Modular (Version Completa)

## Lectura rapida

- bloques `core`, `json-processing`, `users` y `repository`
- submodulos como `about`, `health`, `info`, `validate`, `format`, `analyze`, `compare`
- acciones tipo `about-get/`, `info-post/`, `validate-post/`, `compare-post/`
- dentro de cada accion: `controller`, `dto` y `service`
- el wiring queda en `*.module.js`

Esta version completa muestra una arquitectura modular mas profunda para `adminJsonLens`.

La idea es mantener el sistema organizado por modulos, pero dejando preparada una estructura mejor para crecer cuando aparecen:

- mas endpoints
- mas equipos o colaboradores
- mas casos de uso por modulo
- persistencia de historial o usuarios
- validaciones mas finas
- pruebas mas amplias

Aqui el backend sigue resolviendo estas capacidades:

- validar JSON
- formatear JSON
- analizar estructura JSON
- comparar documentos JSON
- exponer informacion tecnica del servicio
- preparar gestion de usuarios e historial

---

## 1. Idea central

En una arquitectura modular completa, cada modulo no solo agrupa endpoints.
Tambien agrupa submodulos, acciones y piezas internas con una frontera clara.

En este ejemplo, `adminJsonLens` puede organizarse alrededor de estos bloques:

- `core`
- `json-processing`
- `users`
- `repository`

La regla sigue siendo:

- cada modulo tiene una responsabilidad clara
- cada modulo controla sus controladores, servicios, DTOs y entidades
- `shared/` guarda solo piezas realmente transversales
- `index.module.js` compone el backend completo

---

## 2. Estructura modular completa recomendada

```text
adminJsonLens/
|-- docs/                                        # Documentacion tecnica del backend
|-- src/
|   |-- app.js                                   # Configura Express, middlewares y montaje global
|   |-- server.js                                # Punto de entrada del servidor
|   |
|   |-- config/
|   |   |-- config.index.js                      # Configuracion principal del entorno
|   |   `-- env.js                               # Lectura y normalizacion de variables
|   |
|   |-- shared/
|   |   |-- errors/
|   |   |   `-- AppError.js                      # Error reutilizable de toda la app
|   |   |
|   |   |-- middlewares/
|   |   |   |-- requestLogger.js                 # Logging comun de peticiones
|   |   |   |-- validatePayload.js               # Validacion comun de payloads
|   |   |   `-- errorHandler.js                  # Manejo global de errores
|   |   |
|   |   |-- utils/
|   |   |   |-- safeJsonParse.js                 # Parse seguro de entrada JSON
|   |   |   |-- prettyJson.js                    # Formateo reutilizable
|   |   |   |-- inspectJsonShape.js              # Analisis de estructura y tipos
|   |   |   |-- flattenJsonKeys.js               # Aplana claves para inspeccion
|   |   |   `-- buildJsonDiff.js                 # Construye diferencias legibles
|   |   |
|   |   `-- http/
|   |       `-- sendJsonResponse.js              # Respuesta HTTP uniforme
|   |
|   |-- modules/
|   |   |-- index.module.js                      # Registro principal de modulos
|   |   |
|   |   |-- core/
|   |   |   |-- core.module.js                   # Agrupa modulos base del backend
|   |   |   |
|   |   |   |-- about/
|   |   |   |   |-- about.module.js              # Wiring del modulo about
|   |   |   |   `-- about-get/
|   |   |   |       |-- about.controller.js      # Controller GET /about
|   |   |   |       `-- about.service.js         # Datos descriptivos del servicio
|   |   |   |
|   |   |   |-- health/
|   |   |   |   |-- health.module.js             # Wiring del modulo health
|   |   |   |   `-- health-get/
|   |   |   |       |-- health.controller.js     # Controller GET /health
|   |   |   |       `-- health.service.js        # Estado de salud y uptime
|   |   |   |
|   |   |   `-- info/
|   |   |       |-- info.module.js               # Wiring del modulo info
|   |   |       |-- info-get/
|   |   |       |   |-- info.controller.js       # Controller GET /info
|   |   |       |   `-- info.service.js          # Metadatos y versionado
|   |   |       `-- info-post/
|   |   |           |-- info-create.controller.js # Controller POST /info
|   |   |           |-- info-create.dto.js       # Payload de alta de informacion
|   |   |           `-- info-create.service.js   # Logica de creacion de info
|   |   |
|   |   |-- json-processing/
|   |   |   |-- json-processing.module.js        # Agrupa el procesamiento JSON
|   |   |   |
|   |   |   |-- validate/
|   |   |   |   |-- validate.module.js           # Wiring del submodulo validate
|   |   |   |   `-- validate-post/
|   |   |   |       |-- validate-json.controller.js # Controller POST /json/validate
|   |   |   |       |-- validate-json.dto.js     # Payload de validacion
|   |   |   |       `-- validate-json.service.js # Verifica sintaxis y estructura
|   |   |   |
|   |   |   |-- format/
|   |   |   |   |-- format.module.js             # Wiring del submodulo format
|   |   |   |   `-- format-post/
|   |   |   |       |-- format-json.controller.js # Controller POST /json/format
|   |   |   |       |-- format-json.dto.js       # Payload de formateo
|   |   |   |       `-- format-json.service.js   # Pretty print y normalizacion
|   |   |   |
|   |   |   |-- analyze/
|   |   |   |   |-- analyze.module.js            # Wiring del submodulo analyze
|   |   |   |   `-- analyze-post/
|   |   |   |       |-- analyze-json.controller.js # Controller POST /json/analyze
|   |   |   |       |-- analyze-json.dto.js      # Payload de analisis
|   |   |   |       `-- analyze-json.service.js  # Claves, tipos y estructura
|   |   |   |
|   |   |   `-- compare/
|   |   |       |-- compare.module.js            # Wiring del submodulo compare
|   |   |       `-- compare-post/
|   |   |           |-- compare-json.controller.js # Controller POST /json/compare
|   |   |           |-- compare-json.dto.js      # Payload de comparacion
|   |   |           `-- compare-json.service.js  # Diferencias entre documentos
|   |   |
|   |   |-- users/
|   |   |   |-- users.module.js                  # Agrupa endpoints de usuarios
|   |   |   |-- user-entity/
|   |   |   |   `-- user.entity.js               # Estructura de usuario del sistema
|   |   |   |-- users-get/
|   |   |   |   |-- users.controller.js          # Controller GET /users
|   |   |   |   `-- users.service.js             # Listado y consulta de usuarios
|   |   |   `-- users-post/
|   |   |       |-- user-create.controller.js    # Controller POST /users
|   |   |       |-- user-create.dto.js           # Payload de alta de usuario
|   |   |       `-- user-create.service.js       # Logica de alta de usuario
|   |   |
|   |   `-- repository/
|   |       |-- repository.module.js             # Capa modular de acceso a datos
|   |       |-- modules/
|   |       |   |-- user.repository.js           # Persistencia de usuarios
|   |       |   |-- analysis-history.repository.js # Historial de analisis JSON
|   |       |   `-- compare-history.repository.js # Historial de comparaciones
|   |       `-- adapters/
|   |           `-- json-file.repository.js      # Adaptador a almacenamiento JSON local
|   |
|   |-- routes/
|   |   `-- index.routes.js                      # Entrada opcional para montaje global de rutas
|   |
|   `-- utils/
|       `-- startupBanner.js                     # Banner o salida de arranque
|
|-- tests/
|   |-- unit/
|   |   |-- core/
|   |   |   |-- about.service.test.js
|   |   |   `-- health.service.test.js
|   |   |-- json-processing/
|   |   |   |-- validate-json.service.test.js
|   |   |   |-- format-json.service.test.js
|   |   |   |-- analyze-json.service.test.js
|   |   |   `-- compare-json.service.test.js
|   |   `-- users/
|   |       `-- users.service.test.js
|   |
|   `-- integration/
|       |-- health.routes.test.js
|       |-- info.routes.test.js
|       |-- validate-json.routes.test.js
|       |-- format-json.routes.test.js
|       |-- analyze-json.routes.test.js
|       `-- compare-json.routes.test.js
|
|-- .env                                        # Variables locales del entorno
|-- env.example                                 # Plantilla de variables de entorno
|-- index.html                                  # Vista simple o placeholder local si aplica
|-- package.json                                # Scripts y dependencias
|-- package-lock.json                           # Lock de dependencias
`-- README.md                                   # Guia general del servicio
```

---

## 3. Como leer esta version

En esta arquitectura, el backend se entiende por bloques funcionales:

- `core` contiene informacion base del servicio
- `json-processing` contiene el corazon funcional del producto
- `users` prepara gestion de usuarios del sistema
- `repository` concentra persistencia e historial

Cada bloque puede contener submodulos internos con acciones propias.

Por ejemplo:

```text
json-processing/
|-- validate/
|-- format/
|-- analyze/
`-- compare/
```

Eso permite crecer sin mezclar operaciones distintas dentro del mismo archivo.

---

## 4. Patron interno de un modulo completo

En esta version completa, un modulo puede seguir un patron como este:

```text
module-name/
|-- module-name.module.js                    # Ensamblado del modulo
|-- action-a/
|   |-- action-a.controller.js               # Adaptador HTTP
|   |-- action-a.dto.js                      # Contrato de entrada
|   `-- action-a.service.js                  # Logica de la accion
`-- action-b/
    |-- action-b.controller.js
    |-- action-b.dto.js
    `-- action-b.service.js
```

Si el modulo necesita entidades o repositorios propios, se pueden agregar carpetas dedicadas dentro del mismo modulo o conectarlo con `repository/`.

---

## 5. Flujo habitual

Un flujo normal en esta version completa puede quedar asi:

```text
JsonLens frontend
  -> index.module.js
  -> modulo o submodulo correspondiente
  -> controller de la accion
  -> service de la accion
  -> utils compartidas o repository si hace falta
  -> response JSON lista para la UI
```

---

## 6. Cuando usar esta version

Usa esta version cuando:

- ya tienes varios modulos y submodulos
- quieres separar acciones por endpoint
- necesitas historial, usuarios o persistencia adicional
- quieres tests mas organizados
- quieres una arquitectura modular lista para crecer bastante

---

## 7. Resumen rapido

La version completa se resume asi:

- organiza el backend por modulos funcionales y submodulos internos
- deja claro donde vive cada endpoint y cada accion
- separa `core`, `json-processing`, `users` y `repository`
- prepara el sistema para crecer sin perder legibilidad

En corto:

```text
Modular = el backend se divide por capacidades del sistema
Completo = cada modulo puede crecer con acciones, entidades y persistencia propia
```
