# Arquitectura Modular (Version MVP)

## Lectura rapida

- bloques `core` y `json-workspace`
- submodulos como `home`, `about`, `editor`, `analyze`, `compare`
- acciones tipo `home-view/`, `editor-view/`, `compare-view/`
- dentro de cada accion: `page`, `hook` y `service`
- el wiring queda en `*.module.ts`

Esta version MVP muestra una forma simple de organizar `JsonLensWeb` como una app web modular.

La idea es que el MVP ya se lea como una arquitectura final en pequeño y no como una estructura temporal desordenada.

---

## 1. Idea central

En una arquitectura modular frontend, el proyecto se organiza por bloques funcionales de UI y negocio cercano a la pantalla.

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
jsonLensWeb/
|-- index.html                                 # Entrada HTML
|-- package.json                               # Dependencias y scripts
|-- vite.config.ts                             # Configuracion del bundler
|-- README.md                                  # Guia general
|
|-- src/
|   |-- main.tsx                               # Bootstrap principal
|   |-- App.tsx                                # Componente raiz
|   |
|   |-- shared/
|   |   |-- components/
|   |   |   `-- AppShell.tsx                   # Layout global de la app
|   |   |-- lib/
|   |   |   |-- httpClient.ts                  # Cliente HTTP comun
|   |   |   `-- browserStorage.ts              # Storage compartido
|   |   `-- ui/
|   |       `-- EmptyState.tsx                 # Estado vacio reutilizable
|   |
|   `-- modules/
|       |-- index.module.ts                    # Registro principal de bloques
|       |
|       |-- core/
|       |   |-- core.module.ts                 # Wiring del bloque core
|       |   |
|       |   |-- home/
|       |   |   |-- home.module.ts             # Wiring del submodulo home
|       |   |   `-- home-view/
|       |   |       |-- home.page.tsx          # Vista principal
|       |   |       |-- home.hook.ts           # Estado y eventos
|       |   |       `-- home.service.ts        # Datos de la vista
|       |   |
|       |   `-- about/
|       |       |-- about.module.ts            # Wiring del submodulo about
|       |       `-- about-view/
|       |           |-- about.page.tsx         # Vista about
|       |           |-- about.hook.ts          # Estado de la vista
|       |           `-- about.service.ts       # Datos de la vista
|       |
|       `-- json-workspace/
|           |-- json-workspace.module.ts       # Wiring del bloque principal
|           |
|           |-- editor/
|           |   |-- editor.module.ts           # Wiring del submodulo editor
|           |   `-- editor-view/
|           |       |-- editor.page.tsx        # Vista de edicion
|           |       |-- editor.hook.ts         # Estado del editor
|           |       `-- editor.service.ts      # Operaciones del editor
|           |
|           |-- analyze/
|           |   |-- analyze.module.ts          # Wiring del submodulo analyze
|           |   `-- analyze-view/
|           |       |-- analyze.page.tsx       # Vista de analisis
|           |       |-- analyze.hook.ts        # Estado del analisis
|           |       `-- analyze.service.ts     # Logica de analisis
|           |
|           `-- compare/
|               |-- compare.module.ts          # Wiring del submodulo compare
|               `-- compare-view/
|                   |-- compare.page.tsx       # Vista de comparacion
|                   |-- compare.hook.ts        # Estado del diff
|                   `-- compare.service.ts     # Logica de comparacion
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
- vista o accion
- archivos internos de esa accion

Ejemplo:

```text
editor/
|-- editor.module.ts
`-- editor-view/
    |-- editor.page.tsx
    |-- editor.hook.ts
    `-- editor.service.ts
```

---

## 4. Flujo habitual

Un flujo normal queda asi:

```text
Route
  -> Page
  -> Hook
  -> Service
  -> HTTP client o storage
  -> UI actualizada
```

---

## 5. Cuando usar esta version

Usa esta version cuando:

- la app web esta en MVP
- quieres bloques faciles de leer
- todavia no necesitas demasiada separacion por estado, session o settings

---

## 6. Resumen rapido

La version MVP se resume asi:

- el frontend se divide por bloques funcionales
- cada accion de UI tiene `page`, `hook` y `service`
- el wiring del bloque queda en `*.module.ts`
