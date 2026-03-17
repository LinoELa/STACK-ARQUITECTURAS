## Descripcion General

### Arquitectura frontend con React

Este ejemplo describe una posible arquitectura frontend con React para una aplicacion web de gestion. La estructura separa interfaz, navegacion, estado y consumo de API para que el desarrollo sea mas predecible y facil de ampliar.

## Infraestructura Tecnica

```text
DOSAccessWeb/
|-- src/
|   |-- main.tsx                          # Punto de entrada de Vite + React
|   |-- App.tsx                           # Monta Redux, ThemeProvider, Core, Router y Toaster
|   |-- config/                           # Version, flags, URLs y configuracion global
|   |-- translations/                     # i18n y locales en en/es/de/fr/it
|   |-- assets/                           # Recursos estaticos
|   |-- lib/                              # Constantes y utilidades compartidas
|   |-- components/
|   |   |-- core/                         # Auto-login, initSession y bootstrap global
|   |   |-- router/                       # BrowserRouter, rutas publicas y privadas
|   |   |-- layout/                       # Shell principal, sidebar y secciones
|   |   |-- ui/                           # Componentes base reutilizables (shadcn/radix)
|   |   |-- common/                       # Componentes de negocio compartidos
|   |   |-- theme/                        # Proveedor de tema visual
|   |   |-- utils/                        # Utilidades de interfaz
|   |   `-- deprecated/                   # Componentes legacy aun presentes
|   |-- pages/
|   |   |-- public/                       # Login y reservas publicas
|   |   `-- private/                      # Dashboard, facilities, staff, hotel, settings, etc.
|   |       |-- dashboard/                # Vista principal autenticada
|   |       |-- building/                 # Infraestructura y dispositivos (rutas /facilities/*)
|   |       |-- staff/                    # Usuarios, roles, perfiles y schedulers
|   |       |-- hotel/                    # Check-in, reservas y ocupacion
|   |       |-- audits/                   # Auditoria de aperturas
|   |       |-- marketplace/              # Extensiones
|   |       |-- settings/                 # Ajustes y plantillas de email
|   |       |-- support/                  # Incidencias
|   |       |-- monitoring/               # Colas y monitorizacion
|   |       `-- debug-tools/              # Logs HTTP y herramientas de depuracion
|   |-- data/
|   |   |-- api/
|   |   |   |-- index.ts                  # Punto unico de acceso a proveedores API
|   |   |   |-- service/                  # Cliente Axios principal hacia OSAccessService
|   |   |   |   |-- service-http-client.ts # Token Bearer, tenantId e interceptores HTTP
|   |   |   |   |-- shared/               # DTOs y utilidades comunes
|   |   |   |   |-- local/                # Recursos locales del proveedor service
|   |   |   |   `-- modules/
|   |   |   |       |-- core/             # Buildings, locks, spaces, users, permissions, etc.
|   |   |   |       `-- extensions/       # Auth, staff, hotel, monitoring y debug-tools
|   |   |   |-- cardosaccess/             # Proveedor secundario para CardOSAccess
|   |   |   `-- test/                     # Inicializacion de pruebas de API
|   |   `-- store/
|   |       |-- store.ts                  # Redux Toolkit store, hooks tipados y middleware debug
|   |       `-- reducers/
|   |           |-- session/              # Sesion, tenant y autenticacion
|   |           |-- core/                 # Estado normalizado de entidades principales
|   |           |-- extensions/           # Estado de modulos funcionales adicionales
|   |           |-- abstractions/         # Selectores derivados y vistas compuestas
|   |           `-- common/               # Factories y helpers reutilizables para slices
|   `-- vite-env.d.ts                     # Tipos de Vite
|-- public/                               # Recursos publicos servidos por Vite
|-- dist/                                 # Build de produccion
|-- package.json                          # Scripts y dependencias del frontend
|-- vite.config.ts                        # Puerto 56221 y alias @ -> src
|-- tsconfig.json                         # Configuracion TypeScript base
|-- tsconfig.app.json                     # Configuracion TS de la aplicacion
|-- tsconfig.node.json                    # Configuracion TS para tooling Node
|-- components.json                       # Configuracion de shadcn/ui
|-- ARCHITECTURE.md                       # Documentacion tecnica ampliada del proyecto
`-- README.md                             # Resumen del sistema y puertos del grupo OSAccess
```

## Arquitectura Funcional

```text
DOSAccessWeb
|-- Capa de presentacion
|   |-- pages/                            # Pantallas por dominio funcional
|   `-- components/                       # Layout, UI comun, router y elementos reutilizables
|-- Capa de entrada
|   |-- main.tsx                          # Arranque de React en el DOM
|   `-- App.tsx                           # Ensambla store, tema, core, router y notificaciones
|-- Capa de orquestacion
|   |-- components/core/                  # Auto-login, initSession y bootstrap inicial
|   `-- components/router/                # Control de acceso entre rutas publicas y privadas
|-- Capa de estado
|   `-- data/store/                       # Redux Toolkit, slices por dominio y hooks de acceso
|-- Capa de integracion
|   `-- data/api/service/                 # Axios, casos de uso HTTP, token y tenantId
|-- Capa transversal
|   |-- config/                           # Flags, URLs, version y opciones globales
|   |-- translations/                     # Internacionalizacion
|   `-- lib/                              # Constantes y utilidades
`-- Backend consumido
    `-- OSAccessService                   # API REST en localhost:56220 o service.osaccess.cloud/api
```

## Flujo Principal

```text
Usuario
  -> Pages / Components
  -> Hooks del store
  -> Slices Redux
  -> API service (Axios + interceptores)
  -> OSAccessService
  -> Store actualizado
  -> Re-render de la UI
```
