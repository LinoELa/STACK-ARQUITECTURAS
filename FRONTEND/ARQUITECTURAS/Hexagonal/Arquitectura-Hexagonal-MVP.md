# Arquitectura Hexagonal + Vertical Slicing + Screaming (Version MVP)

## Lectura rapida

- slices `auth`, `workspace` y `compare`
- cada slice tiene `domain`, `application` e `infrastructure`
- la UI vive en `infrastructure` junto con `http`, `storage` y `router`
- pages, hooks y components llaman a casos de uso
- el wiring del slice queda en `container.ts`

Esta version MVP muestra una forma simple y realista de organizar `JsonLensWeb` como una app frontend con Hexagonal, Vertical Slicing y Screaming.

La idea no es meter demasiadas carpetas, sino dejar una base clara para una app web con editor JSON, analisis y comparacion.

---

## 1. Idea central

La estructura debe gritar negocio antes que tecnologia.

Cuando alguien abra `src/`, deberia entender rapido que la app trata de:

- autenticacion
- workspace JSON
- comparacion

y no ver primero carpetas globales como:

- `pages`
- `components`
- `api`
- `hooks`

La regla base es esta:

- cada feature vive junta
- el dominio no depende de React
- la infraestructura adapta UI, HTTP y browser storage

---

## 2. Estructura MVP recomendada

```text
jsonLensWeb/
|-- index.html                                 # Entrada HTML de la app
|-- package.json                               # Scripts y dependencias
|-- vite.config.ts                             # Configuracion del bundler
|-- README.md                                  # Guia general del proyecto
|
|-- src/
|   |-- main.tsx                               # Entrada principal de la app
|   |-- App.tsx                                # Componente raiz
|   |
|   |-- app/
|   |   |-- createApp.tsx                      # Monta providers globales
|   |   `-- router.tsx                         # Router principal de la app
|   |
|   |-- shared/
|   |   |-- domain/
|   |   |   `-- errors/
|   |   |       `-- AppError.ts                # Error reutilizable
|   |   `-- infrastructure/
|   |       |-- http/
|   |       |   `-- apiClient.ts               # Cliente HTTP compartido
|   |       |-- browser-storage/
|   |       |   `-- localStorageClient.ts      # Storage web compartido
|   |       `-- ui/
|   |           `-- layouts/
|   |               `-- AppShell.tsx           # Layout base de la aplicacion
|   |
|   `-- modules/
|       |-- auth/
|       |   |-- domain/
|       |   |   |-- User.ts                    # Entidad de usuario
|       |   |   |-- SessionRepository.ts       # Puerto para sesion remota
|       |   |   `-- TokenStore.ts              # Puerto para token local
|       |   |-- application/
|       |   |   |-- LoginUserUseCase.ts        # Login de usuario
|       |   |   |-- LogoutUserUseCase.ts       # Logout
|       |   |   `-- GetCurrentSessionUseCase.ts # Sesion actual
|       |   `-- infrastructure/
|       |       |-- http/
|       |       |   `-- authApi.ts             # Cliente remoto de auth
|       |       |-- storage/
|       |       |   `-- BrowserTokenStore.ts   # Token en browser storage
|       |       |-- ui/
|       |       |   |-- pages/
|       |       |   |   `-- LoginPage.tsx      # Pantalla de login
|       |       |   |-- components/
|       |       |   |   `-- LoginForm.tsx      # Formulario de login
|       |       |   `-- hooks/
|       |       |       `-- useLoginPage.ts    # Estado de la page
|       |       `-- container.ts               # Wiring del slice
|       |
|       |-- workspace/
|       |   |-- domain/
|       |   |   |-- JsonDocument.ts            # Documento JSON
|       |   |   `-- WorkspaceRepository.ts     # Puerto de datos del workspace
|       |   |-- application/
|       |   |   |-- LoadWorkspaceUseCase.ts    # Carga del workspace
|       |   |   |-- FormatJsonUseCase.ts       # Formateo de JSON
|       |   |   `-- AnalyzeJsonUseCase.ts      # Analisis del documento
|       |   `-- infrastructure/
|       |       |-- http/
|       |       |   `-- workspaceApi.ts        # Cliente remoto del workspace
|       |       |-- ui/
|       |       |   |-- pages/
|       |       |   |   `-- WorkspacePage.tsx  # Vista principal de trabajo
|       |       |   |-- components/
|       |       |   |   |-- JsonEditor.tsx     # Editor principal
|       |       |   |   `-- AnalysisPanel.tsx  # Panel de analisis
|       |       |   `-- hooks/
|       |       |       `-- useWorkspacePage.ts # Estado de la page
|       |       `-- container.ts               # Wiring del slice
|       |
|       `-- compare/
|           |-- domain/
|           |   |-- JsonDiff.ts                # Resultado de comparacion
|           |   `-- CompareRepository.ts       # Puerto de comparacion
|           |-- application/
|           |   `-- CompareDocumentsUseCase.ts # Compara dos documentos
|           `-- infrastructure/
|               |-- http/
|               |   `-- compareApi.ts          # Cliente remoto de comparacion
|               |-- ui/
|               |   |-- pages/
|               |   |   `-- ComparePage.tsx    # Vista de comparacion
|               |   |-- components/
|               |   |   `-- DiffViewer.tsx     # Render del diff
|               |   `-- hooks/
|               |       `-- useComparePage.ts  # Estado de la page
|               `-- container.ts               # Wiring del slice
|
`-- tests/
    |-- auth/
    |-- workspace/
    `-- compare/
```

---

## 3. Regla de dependencias

La direccion correcta es esta:

```text
infrastructure -> application -> domain
```

Eso significa:

- `domain` no conoce React ni Vite
- `application` trabaja con contratos del dominio
- `infrastructure` adapta HTTP, storage, router y UI

---

## 4. Flujo habitual

Un flujo normal en esta arquitectura suele quedar asi:

```text
Route
  -> Page
  -> Hook o controller de UI
  -> Use Case
  -> Domain Port
  -> HTTP client o browser storage
  -> UI state actualizado
```

---

## 5. Cuando usar esta version

Usa esta version cuando:

- la app web esta en MVP
- tienes pocas features principales
- quieres Hexagonal real sin demasiada profundidad
- quieres dejar un camino claro para crecer

---

## 6. Resumen rapido

La version MVP se resume asi:

- `auth`, `workspace` y `compare` son los slices visibles
- cada slice tiene `domain`, `application` e `infrastructure`
- React y HTTP quedan en los bordes
- la UI sigue al negocio y no al reves
