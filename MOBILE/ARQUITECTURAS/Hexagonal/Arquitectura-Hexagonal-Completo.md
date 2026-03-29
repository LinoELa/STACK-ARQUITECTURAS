# Arquitectura Hexagonal + Vertical Slicing + Screaming (Version Completa)

## Lectura rapida

- slices `auth`, `workspace`, `compare`, `history` y `settings`
- cada slice separa `domain`, `application` e `infrastructure`
- `infrastructure` divide `ui`, `http`, `storage`, `state` y `navigation`
- screens, hooks y components llaman a casos de uso y puertos
- el wiring del slice queda en `container.ts`

Esta version completa muestra una arquitectura mas profunda para `JsonLensMobile`.

El objetivo aqui es dejar una base preparada para una app movil con varias screens, mas estado compartido y mas integraciones de plataforma.

---

## 1. Idea central

La arquitectura sigue gritando negocio, pero con una estructura mas preparada para crecer.

La diferencia frente al MVP es que aqui:

- se separan mejor `screens`, `components`, `hooks`, `state` y `navigation`
- hay mas slices funcionales
- cada slice queda listo para crecer sin mezclar UI y negocio

---

## 2. Estructura completa recomendada

```text
jsonLensMobile/
|-- App.tsx                                   # Entrada principal de la app
|-- package.json                              # Dependencias y scripts
|-- app.json                                  # Configuracion general
|-- README.md                                 # Guia general
|
|-- src/
|   |-- app/
|   |   |-- createApp.tsx                     # Monta providers y navigation
|   |   |-- navigation.tsx                    # Navegacion principal
|   |   `-- buildModules.ts                   # Construye slices
|   |
|   |-- shared/
|   |   |-- domain/
|   |   |   `-- errors/
|   |   |       `-- AppError.ts               # Error base de dominio
|   |   `-- infrastructure/
|   |       |-- http/
|   |       |   `-- apiClient.ts              # Cliente HTTP comun
|   |       |-- storage/
|   |       |   `-- secureStorageClient.ts    # Storage seguro comun
|   |       |-- navigation/
|   |       |   `-- RootNavigator.tsx         # Navegacion compartida
|   |       |-- state/
|   |       |   `-- queryClient.ts            # Estado remoto compartido
|   |       `-- ui/
|   |           |-- components/
|   |           |   `-- ScreenShell.tsx       # Shell global de screen
|   |           `-- theme/
|   |               `-- themeProvider.tsx     # Theme global
|   |
|   `-- modules/
|       |-- auth/
|       |   |-- domain/
|       |   |   |-- entities/
|       |   |   |   `-- User.ts               # Entidad usuario
|       |   |   `-- ports/
|       |   |       |-- AuthApi.ts            # Puerto remoto auth
|       |   |       `-- SessionStore.ts       # Puerto de sesion segura
|       |   |-- application/
|       |   |   |-- login/
|       |   |   |   `-- LoginUserUseCase.ts   # Login
|       |   |   |-- logout/
|       |   |   |   `-- LogoutUserUseCase.ts  # Logout
|       |   |   `-- current-session/
|       |   |       `-- GetCurrentSessionUseCase.ts # Sesion actual
|       |   `-- infrastructure/
|       |       |-- http/
|       |       |   `-- authApi.ts            # Cliente remoto auth
|       |       |-- storage/
|       |       |   `-- SecureSessionStore.ts # Sesion segura local
|       |       |-- ui/
|       |       |   |-- screens/
|       |       |   |   `-- LoginScreen.tsx   # Screen login
|       |       |   |-- components/
|       |       |   |   `-- LoginForm.tsx     # Formulario auth
|       |       |   `-- hooks/
|       |       |       `-- useLoginScreen.ts # Estado de la screen
|       |       `-- container.ts              # Wiring del slice
|       |
|       |-- workspace/
|       |   |-- domain/
|       |   |   |-- entities/
|       |   |   |   |-- JsonDocument.ts       # Documento JSON
|       |   |   |   `-- AnalysisResult.ts     # Resultado de analisis
|       |   |   `-- ports/
|       |   |       |-- WorkspaceApi.ts       # Puerto remoto workspace
|       |   |       `-- DraftStore.ts         # Puerto de borradores locales
|       |   |-- application/
|       |   |   |-- load/
|       |   |   |   `-- LoadWorkspaceUseCase.ts # Carga de workspace
|       |   |   |-- format/
|       |   |   |   `-- FormatJsonUseCase.ts  # Formateo
|       |   |   |-- analyze/
|       |   |   |   `-- AnalyzeJsonUseCase.ts # Analisis
|       |   |   `-- save-draft/
|       |   |       `-- SaveDraftUseCase.ts   # Guardado local
|       |   `-- infrastructure/
|       |       |-- http/
|       |       |   `-- workspaceApi.ts       # Cliente remoto workspace
|       |       |-- storage/
|       |       |   `-- DraftSecureStore.ts   # Drafts locales seguros
|       |       |-- state/
|       |       |   `-- workspaceStore.ts     # Estado del workspace
|       |       |-- ui/
|       |       |   |-- screens/
|       |       |   |   `-- WorkspaceScreen.tsx # Screen principal
|       |       |   |-- components/
|       |       |   |   |-- JsonEditor.tsx    # Editor JSON
|       |       |   |   |-- AnalysisCard.tsx  # Tarjeta de analisis
|       |       |   |   `-- Toolbar.tsx       # Acciones rapidas
|       |       |   `-- hooks/
|       |       |       `-- useWorkspaceScreen.ts # Estado de la screen
|       |       `-- container.ts              # Wiring del slice
|       |
|       |-- compare/
|       |   |-- domain/
|       |   |   |-- entities/
|       |   |   |   `-- JsonDiff.ts           # Resultado diff
|       |   |   `-- ports/
|       |   |       `-- CompareApi.ts         # Puerto remoto compare
|       |   |-- application/
|       |   |   |-- compare/
|       |   |   |   `-- CompareDocumentsUseCase.ts # Comparacion
|       |   |   `-- export/
|       |   |       `-- ExportDiffUseCase.ts  # Exportacion del diff
|       |   `-- infrastructure/
|       |       |-- http/
|       |       |   `-- compareApi.ts         # Cliente remoto compare
|       |       |-- ui/
|       |       |   |-- screens/
|       |       |   |   `-- CompareScreen.tsx # Screen compare
|       |       |   |-- components/
|       |       |   |   `-- DiffViewer.tsx    # Render del diff
|       |       |   `-- hooks/
|       |       |       `-- useCompareScreen.ts # Estado de la screen
|       |       `-- container.ts              # Wiring del slice
|       |
|       |-- history/
|       |   |-- domain/
|       |   |   |-- entities/
|       |   |   |   `-- HistoryEntry.ts       # Entrada de historial
|       |   |   `-- ports/
|       |   |       `-- HistoryApi.ts         # Puerto remoto historial
|       |   |-- application/
|       |   |   |-- list/
|       |   |   |   `-- ListHistoryUseCase.ts # Listado
|       |   |   `-- remove/
|       |   |       `-- RemoveHistoryUseCase.ts # Eliminacion
|       |   `-- infrastructure/
|       |       |-- http/
|       |       |   `-- historyApi.ts         # Cliente remoto historial
|       |       |-- ui/
|       |       |   |-- screens/
|       |       |   |   `-- HistoryScreen.tsx # Screen historial
|       |       |   `-- hooks/
|       |       |       `-- useHistoryScreen.ts # Estado de la screen
|       |       `-- container.ts              # Wiring del slice
|       |
|       `-- settings/
|           |-- domain/
|           |   |-- entities/
|           |   |   `-- UserSettings.ts       # Preferencias del usuario
|           |   `-- ports/
|           |       `-- SettingsStore.ts      # Puerto de settings locales
|           |-- application/
|           |   |-- load/
|           |   |   `-- LoadSettingsUseCase.ts # Carga de settings
|           |   `-- save/
|           |       `-- SaveSettingsUseCase.ts # Guardado
|           `-- infrastructure/
|               |-- storage/
|               |   `-- SecureSettingsStore.ts # Settings locales
|               |-- ui/
|               |   |-- screens/
|               |   |   `-- SettingsScreen.tsx # Screen settings
|               |   `-- hooks/
|               |       `-- useSettingsScreen.ts # Estado de la screen
|               `-- container.ts              # Wiring del slice
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

- el dominio no conoce React Native, Expo, storage ni navigation
- la aplicacion trabaja con contratos y casos de uso
- la infraestructura implementa UI, HTTP, storage, platform y estado

---

## 4. Flujo habitual

Un flujo normal suele quedar asi:

```text
Screen
  -> Hook de screen
  -> Use Case
  -> Domain Port
  -> HTTP client o secure storage
  -> Estado de UI
```

---

## 5. Cuando usar esta version

Usa esta version cuando:

- la app movil ya tiene varias screens y estado compartido
- quieres separar mejor screens, hooks, components, storage y navigation
- quieres una base lista para crecer sin reordenar todo despues

---

## 6. Resumen rapido

La version completa se resume asi:

- la app sigue al negocio
- cada slice agrupa dominio, casos de uso y adaptadores
- la UI y la plataforma son infraestructura y no el centro del sistema
- el repo grita `auth`, `workspace`, `compare`, `history` y `settings`
