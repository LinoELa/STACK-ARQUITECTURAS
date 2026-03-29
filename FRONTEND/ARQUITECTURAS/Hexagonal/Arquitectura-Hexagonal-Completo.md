# Arquitectura Hexagonal + Vertical Slicing + Screaming (Version Completa)

## Lectura rapida

- slices `auth`, `workspace`, `compare`, `history` y `settings`
- cada slice separa `domain`, `application` e `infrastructure`
- `infrastructure` divide `ui`, `http`, `storage`, `state` y `router`
- pages, hooks y components llaman a casos de uso y puertos
- el wiring del slice queda en `container.ts`

Esta version completa muestra una arquitectura mas profunda para `JsonLensWeb`.

El objetivo aqui es dejar una base preparada para una app web que ya tiene varias vistas, mas estado compartido y mas integraciones de browser.

---

## 1. Idea central

La arquitectura sigue gritando negocio, pero con una estructura mas preparada para crecer.

La diferencia frente al MVP es que aqui:

- se separan mejor `pages`, `components`, `hooks` y `state`
- hay mas slices funcionales
- cada slice queda listo para crecer sin mezclar UI y negocio

---

## 2. Estructura completa recomendada

```text
jsonLensWeb/
|-- index.html                                 # Entrada HTML de la app
|-- package.json                               # Dependencias y scripts
|-- vite.config.ts                             # Configuracion del bundler
|-- README.md                                  # Guia general del proyecto
|
|-- src/
|   |-- main.tsx                               # Bootstrap principal
|   |-- App.tsx                                # Componente raiz
|   |
|   |-- app/
|   |   |-- createApp.tsx                      # Monta providers y router
|   |   |-- router.tsx                         # Router principal
|   |   `-- buildModules.ts                    # Construye slices
|   |
|   |-- shared/
|   |   |-- domain/
|   |   |   `-- errors/
|   |   |       `-- AppError.ts                # Error base de dominio
|   |   `-- infrastructure/
|   |       |-- http/
|   |       |   `-- apiClient.ts               # Cliente HTTP comun
|   |       |-- browser-storage/
|   |       |   `-- localStorageClient.ts      # Storage comun
|   |       |-- router/
|   |       |   `-- AppRouter.tsx              # Router compartido
|   |       |-- state/
|   |       |   `-- queryClient.ts             # Estado remoto compartido
|   |       `-- ui/
|   |           |-- layouts/
|   |           |   `-- AppShell.tsx           # Layout global
|   |           `-- theme/
|   |               `-- themeProvider.tsx      # Theme global
|   |
|   `-- modules/
|       |-- auth/
|       |   |-- domain/
|       |   |   |-- entities/
|       |   |   |   `-- User.ts                # Entidad usuario
|       |   |   `-- ports/
|       |   |       |-- AuthApi.ts             # Puerto remoto de auth
|       |   |       `-- SessionStore.ts        # Puerto de sesion local
|       |   |-- application/
|       |   |   |-- login/
|       |   |   |   `-- LoginUserUseCase.ts    # Caso de uso de login
|       |   |   |-- logout/
|       |   |   |   `-- LogoutUserUseCase.ts   # Caso de uso de logout
|       |   |   `-- current-session/
|       |   |       `-- GetCurrentSessionUseCase.ts # Sesion actual
|       |   `-- infrastructure/
|       |       |-- http/
|       |       |   `-- authApi.ts             # Cliente remoto auth
|       |       |-- storage/
|       |       |   `-- BrowserSessionStore.ts # Sesion en browser
|       |       |-- ui/
|       |       |   |-- pages/
|       |       |   |   `-- LoginPage.tsx      # Page de login
|       |       |   |-- components/
|       |       |   |   `-- LoginForm.tsx      # Formulario auth
|       |       |   `-- hooks/
|       |       |       `-- useLoginPage.ts    # Estado de la page
|       |       `-- container.ts               # Wiring del slice
|       |
|       |-- workspace/
|       |   |-- domain/
|       |   |   |-- entities/
|       |   |   |   |-- JsonDocument.ts        # Documento JSON
|       |   |   |   `-- AnalysisResult.ts      # Resultado de analisis
|       |   |   `-- ports/
|       |   |       |-- WorkspaceApi.ts        # Puerto remoto del workspace
|       |   |       `-- DraftStore.ts          # Puerto de borradores locales
|       |   |-- application/
|       |   |   |-- load/
|       |   |   |   `-- LoadWorkspaceUseCase.ts # Carga de workspace
|       |   |   |-- format/
|       |   |   |   `-- FormatJsonUseCase.ts   # Formateo
|       |   |   |-- analyze/
|       |   |   |   `-- AnalyzeJsonUseCase.ts  # Analisis
|       |   |   `-- save-draft/
|       |   |       `-- SaveDraftUseCase.ts    # Guardado local
|       |   `-- infrastructure/
|       |       |-- http/
|       |       |   `-- workspaceApi.ts        # Cliente remoto del workspace
|       |       |-- storage/
|       |       |   `-- DraftLocalStorage.ts   # Drafts locales
|       |       |-- state/
|       |       |   `-- workspaceStore.ts      # Estado del workspace
|       |       |-- ui/
|       |       |   |-- pages/
|       |       |   |   `-- WorkspacePage.tsx  # Vista principal
|       |       |   |-- components/
|       |       |   |   |-- JsonEditor.tsx     # Editor JSON
|       |       |   |   |-- AnalysisPanel.tsx  # Panel de analisis
|       |       |   |   `-- Toolbar.tsx        # Acciones rapidas
|       |       |   `-- hooks/
|       |       |       `-- useWorkspacePage.ts # Estado de la page
|       |       `-- container.ts               # Wiring del slice
|       |
|       |-- compare/
|       |   |-- domain/
|       |   |   |-- entities/
|       |   |   |   `-- JsonDiff.ts            # Resultado diff
|       |   |   `-- ports/
|       |   |       `-- CompareApi.ts          # Puerto remoto de compare
|       |   |-- application/
|       |   |   |-- compare/
|       |   |   |   `-- CompareDocumentsUseCase.ts # Comparacion
|       |   |   `-- export/
|       |   |       `-- ExportDiffUseCase.ts   # Exportacion del diff
|       |   `-- infrastructure/
|       |       |-- http/
|       |       |   `-- compareApi.ts          # Cliente remoto compare
|       |       |-- ui/
|       |       |   |-- pages/
|       |       |   |   `-- ComparePage.tsx    # Vista de comparacion
|       |       |   |-- components/
|       |       |   |   `-- DiffViewer.tsx     # Render del diff
|       |       |   `-- hooks/
|       |       |       `-- useComparePage.ts  # Estado de la page
|       |       `-- container.ts               # Wiring del slice
|       |
|       |-- history/
|       |   |-- domain/
|       |   |   |-- entities/
|       |   |   |   `-- HistoryEntry.ts        # Entrada de historial
|       |   |   `-- ports/
|       |   |       `-- HistoryApi.ts          # Puerto remoto historial
|       |   |-- application/
|       |   |   |-- list/
|       |   |   |   `-- ListHistoryUseCase.ts  # Listado
|       |   |   `-- remove/
|       |   |       `-- RemoveHistoryUseCase.ts # Eliminacion
|       |   `-- infrastructure/
|       |       |-- http/
|       |       |   `-- historyApi.ts          # Cliente remoto historial
|       |       |-- ui/
|       |       |   |-- pages/
|       |       |   |   `-- HistoryPage.tsx    # Vista de historial
|       |       |   `-- hooks/
|       |       |       `-- useHistoryPage.ts  # Estado de la page
|       |       `-- container.ts               # Wiring del slice
|       |
|       `-- settings/
|           |-- domain/
|           |   |-- entities/
|           |   |   `-- UserSettings.ts        # Preferencias de usuario
|           |   `-- ports/
|           |       `-- SettingsStore.ts       # Puerto de settings locales
|           |-- application/
|           |   |-- load/
|           |   |   `-- LoadSettingsUseCase.ts # Carga de settings
|           |   `-- save/
|           |       `-- SaveSettingsUseCase.ts # Guardado
|           `-- infrastructure/
|               |-- storage/
|               |   `-- BrowserSettingsStore.ts # Settings en browser
|               |-- ui/
|               |   |-- pages/
|               |   |   `-- SettingsPage.tsx   # Vista de settings
|               |   `-- hooks/
|               |       `-- useSettingsPage.ts # Estado de la page
|               `-- container.ts               # Wiring del slice
|
`-- tests/
    |-- auth/
    |-- workspace/
    |-- compare/
    |-- history/
    `-- settings/
```

---

## 3. Regla de dependencias

La regla sigue siendo:

```text
infrastructure -> application -> domain
```

Eso significa:

- el dominio no conoce React, Query ni router
- la aplicacion trabaja con contratos y casos de uso
- la infraestructura implementa UI, HTTP, storage y estado

---

## 4. Flujo habitual

Un flujo normal suele quedar asi:

```text
Route
  -> Page
  -> Hook de page
  -> Use Case
  -> Domain Port
  -> HTTP client o storage
  -> Estado de UI
```

---

## 5. Cuando usar esta version

Usa esta version cuando:

- la app web ya tiene varias vistas y estado compartido
- quieres separar mejor pages, hooks, components y storage
- quieres una base lista para crecer sin reordenar todo despues

---

## 6. Resumen rapido

La version completa se resume asi:

- el frontend sigue al negocio
- cada slice agrupa dominio, casos de uso y adaptadores
- la UI es infraestructura y no el centro del sistema
- el repo grita `auth`, `workspace`, `compare`, `history` y `settings`
