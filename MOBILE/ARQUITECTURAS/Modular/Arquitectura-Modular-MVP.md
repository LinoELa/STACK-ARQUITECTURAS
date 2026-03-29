# Arquitectura Modular (Version MVP)

## Lectura rapida

- bloques `core` y `json-workspace`
- submodulos como `home`, `about`, `editor`, `analyze`, `compare`
- acciones tipo `home-screen/`, `editor-screen/`, `compare-screen/`
- dentro de cada accion: `screen`, `hook` y `service`
- el wiring queda en `*.module.ts`

Esta version MVP muestra una forma simple de organizar `JsonLensMobile` como una app movil modular.

La idea es que el MVP ya se lea como una arquitectura final en pequeño y no como una estructura temporal desordenada.

---

## 1. Idea central

En una arquitectura modular movil, el proyecto se organiza por bloques funcionales de UX, screens y acciones cercanas a la screen.

En este MVP, la app puede girar alrededor de:

- `core`
- `json-workspace`

Con eso ya cubres:

- home
- about
- editor JSON
- analisis
- comparacion

---

## 2. Estructura MVP recomendada

```text
jsonLensMobile/
|-- App.tsx                                   # Entrada principal
|-- package.json                              # Dependencias y scripts
|-- app.json                                  # Configuracion de la app
|-- README.md                                 # Guia general
|
|-- src/
|   |-- shared/
|   |   |-- components/
|   |   |   `-- ScreenShell.tsx               # Shell base de screen
|   |   |-- lib/
|   |   |   |-- httpClient.ts                 # Cliente HTTP comun
|   |   |   `-- secureStorage.ts              # Storage seguro compartido
|   |   `-- ui/
|   |       `-- EmptyState.tsx                # Estado vacio reutilizable
|   |
|   `-- modules/
|       |-- index.module.ts                   # Registro principal de bloques
|       |
|       |-- core/
|       |   |-- core.module.ts                # Wiring del bloque core
|       |   |
|       |   |-- home/
|       |   |   |-- home.module.ts            # Wiring del submodulo home
|       |   |   `-- home-screen/
|       |   |       |-- home.screen.tsx       # Screen principal
|       |   |       |-- home.hook.ts          # Estado y eventos
|       |   |       `-- home.service.ts       # Datos de la screen
|       |   |
|       |   `-- about/
|       |       |-- about.module.ts           # Wiring del submodulo about
|       |       `-- about-screen/
|       |           |-- about.screen.tsx      # Screen about
|       |           |-- about.hook.ts         # Estado de la screen
|       |           `-- about.service.ts      # Datos de la screen
|       |
|       `-- json-workspace/
|           |-- json-workspace.module.ts      # Wiring del bloque principal
|           |
|           |-- editor/
|           |   |-- editor.module.ts          # Wiring del submodulo editor
|           |   `-- editor-screen/
|           |       |-- editor.screen.tsx     # Screen de edicion
|           |       |-- editor.hook.ts        # Estado del editor
|           |       `-- editor.service.ts     # Operaciones del editor
|           |
|           |-- analyze/
|           |   |-- analyze.module.ts         # Wiring del submodulo analyze
|           |   `-- analyze-screen/
|           |       |-- analyze.screen.tsx    # Screen de analisis
|           |       |-- analyze.hook.ts       # Estado del analisis
|           |       `-- analyze.service.ts    # Logica de analisis
|           |
|           `-- compare/
|               |-- compare.module.ts         # Wiring del submodulo compare
|               `-- compare-screen/
|                   |-- compare.screen.tsx    # Screen de comparacion
|                   |-- compare.hook.ts       # Estado del diff
|                   `-- compare.service.ts    # Logica de comparacion
|
`-- tests/
    |-- core/
    `-- json-workspace/
```

---

## 3. Como leer esta version

La lectura correcta es esta:

- bloque principal
- submodulo
- screen o accion
- archivos internos de esa screen

Ejemplo:

```text
editor/
|-- editor.module.ts
`-- editor-screen/
    |-- editor.screen.tsx
    |-- editor.hook.ts
    `-- editor.service.ts
```

---

## 4. Flujo habitual

Un flujo normal queda asi:

```text
Screen
  -> Hook
  -> Service
  -> HTTP client o secure storage
  -> UI actualizada
```

---

## 5. Cuando usar esta version

Usa esta version cuando:

- la app movil esta en MVP
- quieres bloques faciles de leer
- todavia no necesitas demasiada separacion por session, settings o historial

---

## 6. Resumen rapido

La version MVP se resume asi:

- la app se divide por bloques funcionales
- cada screen o accion tiene `screen`, `hook` y `service`
- el wiring del bloque queda en `*.module.ts`
