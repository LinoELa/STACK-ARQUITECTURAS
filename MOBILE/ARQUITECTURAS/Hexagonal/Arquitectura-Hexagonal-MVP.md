# Arquitectura Hexagonal + Vertical Slicing + Screaming (Version MVP)

## Lectura rapida

- slices `auth`, `workspace` y `compare`
- cada slice tiene `domain`, `application` e `infrastructure`
- la UI movil vive en `infrastructure` junto con `http`, `storage` y `navigation`
- screens, hooks y components llaman a casos de uso
- el wiring del slice queda en `container.ts`

Esta version MVP muestra una forma simple y realista de organizar `JsonLensMobile` como una app movil con Hexagonal, Vertical Slicing y Screaming.

La idea no es meter demasiadas carpetas, sino dejar una base clara para una app movil con editor JSON, analisis y comparacion.

---

## 1. Idea central

La estructura debe gritar negocio antes que tecnologia.

Cuando alguien abra `src/`, deberia entender rapido que la app trata de:

- autenticacion
- workspace JSON
- comparacion

y no ver primero carpetas globales como:

- `screens`
- `components`
- `api`
- `hooks`

La regla base es esta:

- cada feature vive junta
- el dominio no depende de React Native
- la infraestructura adapta UI, HTTP, secure storage y navigation

---

## 2. Estructura MVP recomendada

```text
jsonLensMobile/
|-- App.tsx                                   # Entrada principal de la app
|-- package.json                              # Scripts y dependencias
|-- app.json                                  # Configuracion general de la app
|-- README.md                                 # Guia general del proyecto
|
|-- src/
|   |-- app/
|   |   |-- createApp.tsx                     # Monta providers globales
|   |   `-- navigation.tsx                    # Navegacion principal
|   |
|   |-- shared/
|   |   |-- domain/
|   |   |   `-- errors/
|   |   |       `-- AppError.ts               # Error reutilizable
|   |   `-- infrastructure/
|   |       |-- http/
|   |       |   `-- apiClient.ts              # Cliente HTTP compartido
|   |       |-- storage/
|   |       |   `-- secureStorageClient.ts    # Storage seguro comun
|   |       `-- ui/
|   |           `-- components/
|   |               `-- ScreenShell.tsx       # Shell base de screen
|   |
|   `-- modules/
|       |-- auth/
|       |   |-- domain/
|       |   |   |-- User.ts                   # Entidad de usuario
|       |   |   |-- SessionRepository.ts      # Puerto de sesion remota
|       |   |   `-- TokenStore.ts             # Puerto de token seguro
|       |   |-- application/
|       |   |   |-- LoginUserUseCase.ts       # Login
|       |   |   |-- LogoutUserUseCase.ts      # Logout
|       |   |   `-- GetCurrentSessionUseCase.ts # Sesion actual
|       |   `-- infrastructure/
|       |       |-- http/
|       |       |   `-- authApi.ts            # Cliente remoto auth
|       |       |-- storage/
|       |       |   `-- SecureTokenStore.ts   # Token en storage seguro
|       |       |-- ui/
|       |       |   |-- screens/
|       |       |   |   `-- LoginScreen.tsx   # Screen de login
|       |       |   |-- components/
|       |       |   |   `-- LoginForm.tsx     # Formulario de login
|       |       |   `-- hooks/
|       |       |       `-- useLoginScreen.ts # Estado de la screen
|       |       `-- container.ts              # Wiring del slice
|       |
|       |-- workspace/
|       |   |-- domain/
|       |   |   |-- JsonDocument.ts           # Documento JSON
|       |   |   `-- WorkspaceRepository.ts    # Puerto de datos del workspace
|       |   |-- application/
|       |   |   |-- LoadWorkspaceUseCase.ts   # Carga del workspace
|       |   |   |-- FormatJsonUseCase.ts      # Formateo
|       |   |   `-- AnalyzeJsonUseCase.ts     # Analisis
|       |   `-- infrastructure/
|       |       |-- http/
|       |       |   `-- workspaceApi.ts       # Cliente remoto del workspace
|       |       |-- ui/
|       |       |   |-- screens/
|       |       |   |   `-- WorkspaceScreen.tsx # Screen principal
|       |       |   |-- components/
|       |       |   |   |-- JsonEditor.tsx    # Editor principal
|       |       |   |   `-- AnalysisCard.tsx  # Tarjeta de analisis
|       |       |   `-- hooks/
|       |       |       `-- useWorkspaceScreen.ts # Estado de la screen
|       |       `-- container.ts              # Wiring del slice
|       |
|       `-- compare/
|           |-- domain/
|           |   |-- JsonDiff.ts               # Resultado de comparacion
|           |   `-- CompareRepository.ts      # Puerto de compare
|           |-- application/
|           |   `-- CompareDocumentsUseCase.ts # Compara documentos
|           `-- infrastructure/
|               |-- http/
|               |   `-- compareApi.ts         # Cliente remoto compare
|               |-- ui/
|               |   |-- screens/
|               |   |   `-- CompareScreen.tsx # Screen de comparacion
|               |   |-- components/
|               |   |   `-- DiffViewer.tsx    # Render del diff
|               |   `-- hooks/
|               |       `-- useCompareScreen.ts # Estado de la screen
|               `-- container.ts              # Wiring del slice
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

- `domain` no conoce React Native ni Expo
- `application` trabaja con contratos del dominio
- `infrastructure` adapta HTTP, storage seguro, navigation y UI

---

## 4. Flujo habitual

Un flujo normal suele quedar asi:

```text
Screen
  -> Hook o controller de UI
  -> Use Case
  -> Domain Port
  -> HTTP client o secure storage
  -> UI state actualizado
```

---

## 5. Cuando usar esta version

Usa esta version cuando:

- la app movil esta en MVP
- tienes pocas features principales
- quieres Hexagonal real sin demasiada profundidad
- quieres dejar un camino claro para crecer

---

## 6. Resumen rapido

La version MVP se resume asi:

- `auth`, `workspace` y `compare` son los slices visibles
- cada slice tiene `domain`, `application` e `infrastructure`
- la UI y la plataforma quedan en los bordes
- la app sigue al negocio y no al reves
