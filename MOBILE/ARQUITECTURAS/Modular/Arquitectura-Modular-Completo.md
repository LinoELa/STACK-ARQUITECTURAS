# Arquitectura Modular (Version Completa)

## Lectura rapida

- bloques `core`, `session`, `workspace`, `history` y `settings`
- submodulos como `home`, `about`, `login`, `editor`, `compare`, `history-list`, `theme`
- acciones tipo `login-screen/`, `editor-screen/`, `compare-screen/`, `theme-screen/`
- dentro de cada accion: `screen`, `hook`, `service` y a veces `schema`
- el wiring queda en `*.module.ts`

Esta version completa muestra una arquitectura modular mas profunda para `JsonLensMobile`.

La idea es dejar una base preparada para varias screens, mas estado local y mas integraciones con storage y API.

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
jsonLensMobile/
|-- App.tsx                                   # Entrada principal
|-- package.json                              # Dependencias y scripts
|-- app.json                                  # Configuracion general
|-- README.md                                 # Guia general
|
|-- src/
|   |-- shared/
|   |   |-- components/
|   |   |   |-- ScreenShell.tsx               # Shell general
|   |   |   `-- TopNavigation.tsx             # Navegacion superior
|   |   |-- lib/
|   |   |   |-- httpClient.ts                 # Cliente HTTP comun
|   |   |   |-- secureStorage.ts              # Storage seguro comun
|   |   |   `-- queryClient.ts                # Estado remoto compartido
|   |   `-- ui/
|   |       |-- Loader.tsx                    # Loader global
|   |       `-- EmptyState.tsx                # Estado vacio reutilizable
|   |
|   `-- modules/
|       |-- index.module.ts                   # Registro principal
|       |
|       |-- core/
|       |   |-- core.module.ts                # Wiring del bloque core
|       |   |-- home/
|       |   |   |-- home.module.ts            # Wiring del submodulo home
|       |   |   `-- home-screen/
|       |   |       |-- home.screen.tsx       # Home principal
|       |   |       |-- home.hook.ts          # Estado de la home
|       |   |       `-- home.service.ts       # Datos de la home
|       |   `-- about/
|       |       |-- about.module.ts           # Wiring del submodulo about
|       |       `-- about-screen/
|       |           |-- about.screen.tsx      # Screen about
|       |           |-- about.hook.ts         # Estado de la screen
|       |           `-- about.service.ts      # Datos y contenido
|       |
|       |-- session/
|       |   |-- session.module.ts             # Wiring del bloque session
|       |   |-- login/
|       |   |   |-- login.module.ts           # Wiring de login
|       |   |   `-- login-screen/
|       |   |       |-- login.screen.tsx      # Screen de login
|       |   |       |-- login.hook.ts         # Estado del formulario
|       |   |       |-- login.service.ts      # Login remoto
|       |   |       `-- login.schema.ts       # Validacion del formulario
|       |   `-- logout/
|       |       |-- logout.module.ts          # Wiring de logout
|       |       `-- logout-action/
|       |           |-- logout.service.ts     # Cierre de sesion
|       |           `-- logout.hook.ts        # Evento de salida
|       |
|       |-- workspace/
|       |   |-- workspace.module.ts           # Wiring del bloque workspace
|       |   |-- editor/
|       |   |   |-- editor.module.ts          # Wiring del editor
|       |   |   `-- editor-screen/
|       |   |       |-- editor.screen.tsx     # Screen del editor
|       |   |       |-- editor.hook.ts        # Estado del editor
|       |   |       `-- editor.service.ts     # Operaciones locales
|       |   |-- format/
|       |   |   |-- format.module.ts          # Wiring del formateo
|       |   |   `-- format-action/
|       |   |       |-- format.service.ts     # Formateo remoto o local
|       |   |       `-- format.hook.ts        # Disparo de accion
|       |   |-- analyze/
|       |   |   |-- analyze.module.ts         # Wiring del analisis
|       |   |   `-- analyze-screen/
|       |   |       |-- analyze.screen.tsx    # Screen de analisis
|       |   |       |-- analyze.hook.ts       # Estado del analisis
|       |   |       `-- analyze.service.ts    # Analisis remoto
|       |   `-- compare/
|       |       |-- compare.module.ts         # Wiring de compare
|       |       `-- compare-screen/
|       |           |-- compare.screen.tsx    # Screen de comparacion
|       |           |-- compare.hook.ts       # Estado del diff
|       |           `-- compare.service.ts    # Comparacion remota
|       |
|       |-- history/
|       |   |-- history.module.ts             # Wiring del bloque history
|       |   `-- history-list/
|       |       |-- history-list.module.ts    # Wiring de historial
|       |       `-- history-list-screen/
|       |           |-- history-list.screen.tsx # Screen del historial
|       |           |-- history-list.hook.ts  # Estado del listado
|       |           `-- history-list.service.ts # Consulta de historial
|       |
|       `-- settings/
|           |-- settings.module.ts            # Wiring del bloque settings
|           |-- theme/
|           |   |-- theme.module.ts           # Wiring de tema
|           |   `-- theme-screen/
|           |       |-- theme.screen.tsx      # Screen de tema
|           |       |-- theme.hook.ts         # Estado de preferencias
|           |       `-- theme.service.ts      # Persistencia local
|           `-- preferences/
|               |-- preferences.module.ts     # Wiring de preferencias
|               `-- preferences-screen/
|                   |-- preferences.screen.tsx # Screen de preferencias
|                   |-- preferences.hook.ts   # Estado del formulario
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
- screen o accion
- archivos internos de la accion

Eso permite crecer sin mezclar screens, hooks y llamadas remotas de toda la app en pocas carpetas globales.

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

- la app movil ya tiene varias areas funcionales
- quieres separar mejor session, workspace, history y settings
- quieres una estructura modular lista para crecer bastante

---

## 6. Resumen rapido

La version completa se resume asi:

- la app se divide por bloques funcionales
- cada screen o accion tiene su propia unidad de trabajo
- el wiring del bloque queda encapsulado
- la lectura del repo sigue siendo clara aunque la app crezca
