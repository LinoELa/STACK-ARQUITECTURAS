# Arquitectura Modular (Version Completa)

## Lectura rapida

- bloques `core`, `session`, `workspace`, `history` y `settings`
- submodulos como `home`, `about`, `login`, `editor`, `compare`, `history-list`, `theme`
- acciones tipo `login-view/`, `editor-view/`, `compare-view/`, `theme-view/`
- dentro de cada accion: `page`, `hook`, `service` y a veces `schema`
- el wiring queda en `*.module.ts`

Esta version completa muestra una arquitectura modular mas profunda para `JsonLensWeb`.

La idea es dejar una base preparada para varias vistas, mas estado local y mas integraciones con browser y API.

---

## 1. Idea central

En esta version, la app se organiza por bloques funcionales grandes y por submodulos internos mas finos.

Los bloques principales pueden ser:

- `core`
- `session`
- `workspace`
- `history`
- `settings`

---

## 2. Estructura completa recomendada

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
|   |   |   |-- AppShell.tsx                   # Layout general
|   |   |   `-- TopNavigation.tsx              # Navegacion superior
|   |   |-- lib/
|   |   |   |-- httpClient.ts                  # Cliente HTTP comun
|   |   |   |-- browserStorage.ts              # Storage comun
|   |   |   `-- queryClient.ts                 # Estado remoto compartido
|   |   `-- ui/
|   |       |-- Loader.tsx                     # Loader global
|   |       `-- EmptyState.tsx                 # Estado vacio reutilizable
|   |
|   `-- modules/
|       |-- index.module.ts                    # Registro principal
|       |
|       |-- core/
|       |   |-- core.module.ts                 # Wiring del bloque core
|       |   |-- home/
|       |   |   |-- home.module.ts             # Wiring del submodulo home
|       |   |   `-- home-view/
|       |   |       |-- home.page.tsx          # Home principal
|       |   |       |-- home.hook.ts           # Estado de la home
|       |   |       `-- home.service.ts        # Datos de la home
|       |   `-- about/
|       |       |-- about.module.ts            # Wiring del submodulo about
|       |       `-- about-view/
|       |           |-- about.page.tsx         # Vista about
|       |           |-- about.hook.ts          # Estado de la vista
|       |           `-- about.service.ts       # Datos y contenido
|       |
|       |-- session/
|       |   |-- session.module.ts              # Wiring del bloque session
|       |   |-- login/
|       |   |   |-- login.module.ts            # Wiring de login
|       |   |   `-- login-view/
|       |   |       |-- login.page.tsx         # Vista de login
|       |   |       |-- login.hook.ts          # Estado del formulario
|       |   |       |-- login.service.ts       # Login remoto
|       |   |       `-- login.schema.ts        # Validacion del formulario
|       |   `-- logout/
|       |       |-- logout.module.ts           # Wiring de logout
|       |       `-- logout-action/
|       |           |-- logout.service.ts      # Cierre de sesion
|       |           `-- logout.hook.ts         # Evento de salida
|       |
|       |-- workspace/
|       |   |-- workspace.module.ts            # Wiring del bloque workspace
|       |   |-- editor/
|       |   |   |-- editor.module.ts           # Wiring del editor
|       |   |   `-- editor-view/
|       |   |       |-- editor.page.tsx        # Vista del editor
|       |   |       |-- editor.hook.ts         # Estado del editor
|       |   |       `-- editor.service.ts      # Operaciones locales
|       |   |-- format/
|       |   |   |-- format.module.ts           # Wiring del formateo
|       |   |   `-- format-action/
|       |   |       |-- format.service.ts      # Formateo remoto o local
|       |   |       `-- format.hook.ts         # Disparo de accion
|       |   |-- analyze/
|       |   |   |-- analyze.module.ts          # Wiring del analisis
|       |   |   `-- analyze-view/
|       |   |       |-- analyze.page.tsx       # Vista de analisis
|       |   |       |-- analyze.hook.ts        # Estado del analisis
|       |   |       `-- analyze.service.ts     # Analisis remoto
|       |   `-- compare/
|       |       |-- compare.module.ts          # Wiring de compare
|       |       `-- compare-view/
|       |           |-- compare.page.tsx       # Vista de comparacion
|       |           |-- compare.hook.ts        # Estado del diff
|       |           `-- compare.service.ts     # Comparacion remota
|       |
|       |-- history/
|       |   |-- history.module.ts              # Wiring del bloque history
|       |   `-- history-list/
|       |       |-- history-list.module.ts     # Wiring de historial
|       |       `-- history-list-view/
|       |           |-- history-list.page.tsx  # Vista del historial
|       |           |-- history-list.hook.ts   # Estado del listado
|       |           `-- history-list.service.ts # Consulta de historial
|       |
|       `-- settings/
|           |-- settings.module.ts             # Wiring del bloque settings
|           |-- theme/
|           |   |-- theme.module.ts            # Wiring de tema
|           |   `-- theme-view/
|           |       |-- theme.page.tsx         # Vista de tema
|           |       |-- theme.hook.ts          # Estado de preferencias
|           |       `-- theme.service.ts       # Persistencia local
|           `-- preferences/
|               |-- preferences.module.ts      # Wiring de preferencias
|               `-- preferences-view/
|                   |-- preferences.page.tsx   # Vista de preferencias
|                   |-- preferences.hook.ts    # Estado del formulario
|                   `-- preferences.service.ts # Persistencia local
|
`-- tests/
    |-- core/
    |-- session/
    |-- workspace/
    |-- history/
    `-- settings/
```

---

## 3. Como leer esta version

La lectura correcta es esta:

- bloque principal
- submodulo
- accion o vista
- archivos internos de la accion

Eso permite crecer sin mezclar paginas, hooks y llamadas remotas de toda la app en pocas carpetas globales.

---

## 4. Flujo habitual

Un flujo normal queda asi:

```text
Route
  -> Page
  -> Hook
  -> Service
  -> HTTP client o browser storage
  -> UI actualizada
```

---

## 5. Cuando usar esta version

Usa esta version cuando:

- la app web ya tiene varias areas funcionales
- quieres separar mejor session, workspace, history y settings
- quieres una estructura modular lista para crecer bastante

---

## 6. Resumen rapido

La version completa se resume asi:

- la app se divide por bloques funcionales
- cada vista o accion tiene su propia unidad de trabajo
- el wiring del bloque queda encapsulado
- la lectura del repo sigue siendo clara aunque la app crezca
