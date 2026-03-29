# Arquitectura Modular (Version MVP)

## Lectura rapida

- bloques `core` y `json-processing`
- submodulos como `about`, `health`, `validate`, `format`, `analyze`, `compare`
- acciones tipo `about-get/`, `validate-post/`, `format-post/`, `compare-post/`
- dentro de cada accion: `controller`, `dto` y `service`
- el wiring queda en `*.module.js`

Esta version MVP de `adminJsonLens` es la version basica de la arquitectura modular completa.

La idea importante aqui es esta:

- mantener el mismo estilo mental de la version completa
- usar menos bloques y menos carpetas
- dejar un camino natural de crecimiento

Por eso esta version no usa otra forma distinta.
Simplemente recorta la version completa para un backend pequeno.

---

## 1. Idea central

En este MVP, `adminJsonLens` puede organizarse alrededor de dos bloques principales:

- `core`
- `json-processing`

Con eso ya cubres bien el primer alcance del backend:

- exponer informacion del servicio
- responder healthcheck
- validar JSON
- formatear JSON
- analizar JSON
- comparar JSON

La diferencia con la version completa es que aqui todavia no metemos:

- `users`
- `repository`
- historial
- persistencia dedicada
- demasiadas piezas transversales

Pero la forma general se mantiene.

---

## 2. Estructura modular MVP recomendada

```text
adminJsonLens/
|-- docs/                                        # Documentacion tecnica del backend
|-- src/
|   |-- app.js                                   # Configura Express y middlewares globales
|   |-- server.js                                # Punto de entrada del servidor
|   |
|   |-- config/
|   |   `-- config.index.js                      # Configuracion general del entorno
|   |
|   |-- shared/
|   |   |-- errors/
|   |   |   `-- AppError.js                      # Error reutilizable del backend
|   |   |-- middlewares/
|   |   |   |-- requestLogger.js                 # Logging basico de peticiones
|   |   |   `-- errorHandler.js                  # Manejo global de errores
|   |   `-- utils/
|   |       |-- safeJsonParse.js                 # Parse seguro de JSON
|   |       |-- prettyJson.js                    # Pretty print reutilizable
|   |       |-- inspectJsonShape.js              # Analisis de estructura
|   |       `-- buildJsonDiff.js                 # Construccion de diferencias
|   |
|   `-- modules/
|       |-- index.module.js                      # Registro principal de modulos
|       |
|       |-- core/
|       |   |-- core.module.js                   # Agrupa modulos base del backend
|       |   |
|       |   |-- about/
|       |   |   |-- about.module.js              # Wiring del modulo about
|       |   |   `-- about-get/
|       |   |       |-- about.controller.js      # Controller GET /about
|       |   |       `-- about.service.js         # Informacion del servicio
|       |   |
|       |   `-- health/
|       |       |-- health.module.js             # Wiring del modulo health
|       |       `-- health-get/
|       |           |-- health.controller.js     # Controller GET /health
|       |           `-- health.service.js        # Estado de salud y uptime
|       |
|       `-- json-processing/
|           |-- json-processing.module.js        # Agrupa operaciones JSON
|           |
|           |-- validate/
|           |   |-- validate.module.js           # Wiring del submodulo validate
|           |   `-- validate-post/
|           |       |-- validate-json.controller.js # Controller POST /json/validate
|           |       |-- validate-json.dto.js     # Payload esperado para validar JSON
|           |       `-- validate-json.service.js # Valida sintaxis y estructura
|           |
|           |-- format/
|           |   |-- format.module.js             # Wiring del submodulo format
|           |   `-- format-post/
|           |       |-- format-json.controller.js # Controller POST /json/format
|           |       |-- format-json.dto.js       # Payload esperado para formatear
|           |       `-- format-json.service.js   # Pretty print y normalizacion
|           |
|           |-- analyze/
|           |   |-- analyze.module.js            # Wiring del submodulo analyze
|           |   `-- analyze-post/
|           |       |-- analyze-json.controller.js # Controller POST /json/analyze
|           |       |-- analyze-json.dto.js      # Payload esperado para analizar
|           |       `-- analyze-json.service.js  # Claves, tipos y profundidad
|           |
|           `-- compare/
|               |-- compare.module.js            # Wiring del submodulo compare
|               `-- compare-post/
|                   |-- compare-json.controller.js # Controller POST /json/compare
|                   |-- compare-json.dto.js      # Payload esperado para comparar
|                   `-- compare-json.service.js  # Diferencias legibles entre documentos
|
|-- tests/
|   |-- core/
|   |   |-- about.service.test.js               # Test del modulo about
|   |   `-- health.service.test.js              # Test del modulo health
|   `-- json-processing/
|       |-- validate-json.service.test.js       # Test de validacion
|       |-- format-json.service.test.js         # Test de formateo
|       |-- analyze-json.service.test.js        # Test de analisis
|       `-- compare-json.service.test.js        # Test de comparacion
|
|-- .env.example                                 # Variables de entorno de ejemplo
|-- package.json                                 # Scripts y dependencias
|-- package-lock.json                            # Lock de dependencias
`-- README.md                                    # Guia general del backend
```

---

## 3. Como leer esta version

Esta version MVP se lee igual que la completa, solo que con menos piezas.

Los bloques principales son:

- `core` para informacion base del servicio
- `json-processing` para el corazon funcional del producto

Dentro de cada bloque, la unidad real de trabajo es el submodulo o la accion.

Por ejemplo:

```text
validate/
|-- validate.module.js
`-- validate-post/
    |-- validate-json.controller.js
    |-- validate-json.dto.js
    `-- validate-json.service.js
```

Eso hace que el MVP ya se vea como una version pequena de la completa.

---

## 4. Patron interno del MVP

En esta version MVP, un modulo o submodulo suele seguir este patron:

```text
module-name/
|-- module-name.module.js                    # Ensamblado del modulo
`-- action-name/
    |-- action-name.controller.js            # Adaptacion HTTP
    |-- action-name.dto.js                   # Contrato de entrada
    `-- action-name.service.js               # Logica de la accion
```

### `*.module.js`

Se encarga de registrar el bloque y conectar sus acciones.

En este MVP, el wiring va aqui.
No hace falta abrir una carpeta extra solo para `routes.js` si todavia no aporta valor.

### `*.controller.js`

Recibe la request y delega al servicio correcto.

### `*.service.js`

Contiene la logica funcional de la accion.

### `*.dto.js`

Define el payload esperado cuando la accion necesita entrada estructurada.

---

## 5. Flujo habitual

En esta arquitectura modular MVP, un flujo normal queda asi:

```text
JsonLens frontend
  -> index.module.js
  -> core o json-processing
  -> controller de la accion
  -> service de la accion
  -> shared/utils si hace falta
  -> response JSON
```

---

## 6. Cuando usar esta version

Usa esta version cuando:

- el proyecto esta en MVP
- quieres una arquitectura modular clara desde el primer dia
- todavia no necesitas usuarios ni persistencia dedicada
- quieres que el MVP ya tenga forma de arquitectura final
- quieres crecer despues sin rehacer la estructura mental

---

## 7. Resumen rapido

La version MVP se resume asi:

- usa la misma idea que la version completa
- mantiene `core` y `json-processing`
- organiza cada accion en controller, dto y service
- deja el wiring en `*.module.js`
- evita separar de mas mientras el sistema sigue pequeno

En corto:

```text
Modular = el backend se divide por capacidades
MVP = la misma base que la completa, pero con menos bloques
```
