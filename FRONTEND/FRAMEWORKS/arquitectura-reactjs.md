
# Índice

1. [Infraestructura Técnica, mapa de la infraestructura](#infraestructura-técnica)
2. [Versión funcional y no tan detallada](#versión-funcional-y-no-tan-detallada)
3. [Arquitectura funcional](#arquitectura-funcional)
4. [Descripción general](#descripción-general)
5. [Información complementaria](#información-complementaria)

## Arquitectura funcional

```text
DOSAccessWeb/
|-- public/                         # Recursos públicos
|-- src/
|   |-- config/                     # Configuración global
|   |-- translations/               # Idiomas
|   |-- assets/                     # Recursos importados
|   |-- lib/                        # Utilidades, hooks, helpers, constantes y tipos
|   |-- components/                 # Componentes reutilizables
|   |-- pages/                      # Páginas públicas y privadas
|   |-- data/                       # API y estado global
|   |-- main.tsx                    # Entrada de React
|   |-- App.tsx                     # App principal
|   `-- vite-env.d.ts               # Tipos de Vite
|-- dist/                           # Build
|-- package.json
|-- vite.config.ts
|-- tsconfig.json
|-- components.json
|-- ARCHITECTURE.md
`-- README.md
```

## Descripción general

### Arquitectura frontend con React

Este ejemplo describe una posible arquitectura frontend con React para una aplicacion web de gestion. La estructura separa interfaz, navegacion, estado y consumo de API para que el desarrollo sea mas predecible y facil de ampliar.

## Información complementaria

Infomacion estructura más completa y ordenada, pensada para React, Vite, TypeScript, Redux y API modular.

Flujo completo recomendado:

Ejemplo práctico de separación:

```text
src/lib/utils/date.utils.ts
```

```ts
export function formatDate(value: string | Date): string {
  return new Intl.DateTimeFormat("es-ES").format(new Date(value));
}
```

### Utils

Esto es `utils` porque es genérico. 
No sabe nada de usuarios, edificios, locks ni permisos.

```text
src/lib/helpers/permissions.helpers.ts
```

```ts
export function canAccessRoute(
  userPermissions: string[],
  requiredPermission?: string,
): boolean {
  if (!requiredPermission) return true;

  return userPermissions.includes(requiredPermission);
}
```
### Helper
Esto es `helper` porque ya tiene lógica de negocio: permisos.

```text
src/lib/hooks/useDebounce.ts
```

```ts
import { useEffect, useState } from "react";

export function useDebounce<T>(value: T, delay = 300): T {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const timeoutId = window.setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    return () => window.clearTimeout(timeoutId);
  }, [value, delay]);

  return debouncedValue;
}
```
### Hook Global
Esto es hook global porque vale para cualquier módulo.

```text
src/pages/private/building/locks/hooks/useLocksFilters.ts
```

```ts
import { useMemo, useState } from "react";
import { useDebounce } from "@/lib/hooks";

export function useLocksFilters() {
  const [search, setSearch] = useState("");
  const [status, setStatus] = useState<string | null>(null);

  const debouncedSearch = useDebounce(search, 300);

  const filters = useMemo(() => {
    return {
      search: debouncedSearch,
      status,
    };
  }, [debouncedSearch, status]);

  return {
    search,
    setSearch,
    status,
    setStatus,
    filters,
  };
}
```
### Hook Privado
Esto es hook privado de entidad porque solo tiene sentido dentro de `locks`.

```text
src/data/api/service/modules/core/locks/locks.service.ts
```

```ts
import { serviceHttpClient } from "../../../service-http-client";
import type { LockDto } from "./locks.dto";

export async function getLocks(): Promise<LockDto[]> {
  const response = await serviceHttpClient.get<LockDto[]>("/core/locks");
  return response.data;
}

export async function openLock(lockId: string): Promise<void> {
  await serviceHttpClient.post(`/core/locks/${lockId}/open`);
}
```
### Service
Esto es `service` porque habla con backend.

```text
src/data/store/reducers/core/locks/locks.thunks.ts
```

```ts
import { createAsyncThunk } from "@reduxjs/toolkit";
import { getLocks } from "@/data/api/service/modules/core/locks";

export const loadLocksThunk = createAsyncThunk(
  "core/locks/loadLocks",
  async () => {
    return await getLocks();
  },
);
```
### Thunk
Esto es thunk porque conecta Redux con el service.

```text
src/pages/private/building/locks/LocksPage.tsx
```

```tsx
import { useEffect } from "react";
import { useAppDispatch, useAppSelector } from "@/data/store/hooks";
import { loadLocksThunk } from "@/data/store/reducers/core/locks/locks.thunks";
import { selectLocks } from "@/data/store/reducers/core/locks/locks.selectors";
import { useLocksFilters } from "./hooks/useLocksFilters";

export function LocksPage() {
  const dispatch = useAppDispatch();
  const locks = useAppSelector(selectLocks);
  const { search, setSearch, filters } = useLocksFilters();

  useEffect(() => {
    dispatch(loadLocksThunk());
  }, [dispatch]);

  return (
    <section>
      <input
        value={search}
        onChange={(event) => setSearch(event.target.value)}
        placeholder="Buscar cerradura"
      />

      <pre>{JSON.stringify({ locks, filters }, null, 2)}</pre>
    </section>
  );
}
```

## Regla clara para decidir dónde va cada cosa:

```text
utils
Funciones puras, genéricas, sin React, sin API y sin negocio fuerte.

helpers
Funciones auxiliares con lógica de negocio o decisiones de la app.

hooks globales
Hooks reutilizables en varios módulos.

hooks privados
Hooks que solo sirven para una página, entidad o módulo concreto.

services
Funciones que llaman al backend o a proveedores externos.

store
Estado global, slices, thunks y selectors.

components/ui
Componentes visuales base.

components/common
Componentes reutilizables con sentido de negocio.

pages/private/*/components
Componentes que solo pertenecen a una página privada concreta.
```

Mi recomendación final: usa `lib` para código compartido de verdad, usa `pages/.../hooks` para lógica específica de pantalla, y deja todos los `services` dentro de `data/api`, no en `lib`.

## Infraestructura Técnica

```text

DOSAccessWeb/
|-- public/                              # Recursos públicos servidos directamente por Vite
|   |-- favicon.ico                      # Icono principal de la aplicación
|   |-- logo.png                         # Logo público si se usa desde HTML o rutas directas
|   |-- manifest.json                    # Configuración PWA si aplica
|   `-- assets/                          # Recursos públicos que no se importan desde código
|
|-- src/
|   |-- config/                          # Configuración global de la aplicación
|   |   |-- app.config.ts                 # Nombre de app, versión, idioma por defecto, tenant por defecto, etc.
|   |   |-- env.config.ts                 # Lectura y validación de variables de entorno de Vite
|   |   |-- api.config.ts                 # URLs base, timeouts HTTP, headers globales
|   |   |-- flags.config.ts               # Feature flags: activar/desactivar módulos
|   |   `-- index.ts                      # Export centralizado de config
|   |
|   |-- translations/                    # i18n y locales
|   |   |-- i18n.ts                       # Inicialización de i18next
|   |   |-- locales.ts                    # Idiomas soportados
|   |   |-- es/                           # Traducciones en español
|   |   `-- en/                           # Traducciones en inglés
|   |
|   |-- assets/                          # Recursos estáticos importados desde código
|   |   |-- images/                       # Imágenes usadas desde componentes
|   |   |-- icons/                        # Iconos propios del frontend
|   |   `-- styles/                       # Estilos globales o recursos CSS
|   |
|   |-- lib/                             # Código compartido que no depende directamente de una página
|   |   |-- constants/                    # Constantes globales reutilizables
|   |   |   |-- app.constants.ts            # Constantes generales de aplicación
|   |   |   |-- routes.constants.ts         # Paths internos: /login, /facilities, /settings, etc.
|   |   |   |-- permissions.constants.ts    # Permisos técnicos del frontend
|   |   |   |-- roles.constants.ts          # Roles conocidos: admin, staff, guest, etc.
|   |   |   |-- storage.constants.ts        # Keys de localStorage/sessionStorage
|   |   |   |-- query-keys.constants.ts     # Keys para cache, filtros o queries
|   |   |   `-- index.ts                   # Export centralizado de constantes
|   |   |
|   |   |-- utils/                        # Funciones puras, genéricas y sin lógica de negocio fuerte
|   |   |   |-- date.utils.ts               # formatDate(), parseDate(), isExpired()
|   |   |   |-- string.utils.ts             # normalizeText(), capitalize(), slugify()
|   |   |   |-- number.utils.ts             # clamp(), toPercentage(), formatNumber()
|   |   |   |-- array.utils.ts              # uniqueBy(), groupBy(), sortByName()
|   |   |   |-- object.utils.ts             # removeEmptyValues(), deepClone()
|   |   |   |-- url.utils.ts                # buildQueryParams(), getUrlParam()
|   |   |   |-- validation.utils.ts         # isValidEmail(), isValidPhone()
|   |   |   `-- index.ts                   # Export centralizado de utils
|   |   |
|   |   |-- helpers/                      # Lógica auxiliar con más contexto de negocio
|   |   |   |-- auth.helpers.ts             # isAuthenticated(), hasValidSession()
|   |   |   |-- permissions.helpers.ts      # canAccessRoute(), canManageUsers()
|   |   |   |-- api-error.helpers.ts        # mapApiErrorToMessage(), isUnauthorizedError()
|   |   |   |-- tenant.helpers.ts           # resolveTenantId(), getTenantFromSession()
|   |   |   |-- routes.helpers.ts           # resolveDefaultRoute(), buildPrivatePath()
|   |   |   |-- user.helpers.ts             # getUserDisplayName(), getUserInitials()
|   |   |   `-- index.ts                   # Export centralizado de helpers
|   |   |
|   |   |-- hooks/                        # Hooks globales reutilizables en toda la app
|   |   |   |-- useDebounce.ts              # Retrasa un valor antes de actualizarlo
|   |   |   |-- useLocalStorage.ts          # Lee/escribe en localStorage de forma segura
|   |   |   |-- usePrevious.ts              # Guarda el valor anterior de una variable
|   |   |   |-- useBoolean.ts               # Maneja estados true/false
|   |   |   |-- useMounted.ts               # Detecta si el componente sigue montado
|   |   |   |-- useWindowSize.ts            # Devuelve ancho/alto de ventana
|   |   |   `-- index.ts                   # Export centralizado de hooks globales
|   |   |
|   |   `-- types/                       # Tipos compartidos genéricos
|   |       |-- common.types.ts             # Nullable, Option, ID, ApiStatus, etc.
|   |       |-- route.types.ts              # Tipos comunes de rutas
|   |       `-- index.ts                  # Export centralizado de tipos
|   |
|   |-- components/                      # Componentes reutilizables y estructura visual
|   |   |-- core/                         # Bootstrap global de la app
|   |   |-- router/                       # BrowserRouter, rutas públicas y privadas
|   |   |-- layout/                       # Shell principal, sidebar, header y contenedores
|   |   |-- ui/                           # Componentes base reutilizables, shadcn/radix
|   |   |-- common/                       # Componentes de negocio compartidos
|   |   |-- theme/                        # Proveedor y configuración visual del tema
|   |   `-- deprecated/                  # Componentes legacy pendientes de refactor
|   |
|   |-- pages/                          # Páginas públicas y privadas
|   |   |-- public/                       # Login y reservas públicas
|   |   `-- private/                     # Dashboard, building, staff, hotel, settings, etc.
|   |
|   |-- data/                           # Acceso a datos, API y estado global
|   |   |-- api/                          # Proveedores HTTP y services
|   |   `-- store/                       # Redux Toolkit store, slices, thunks y selectors
|   |
|   |-- main.tsx                        # Punto de entrada de Vite + React. Renderiza App en el DOM
|   |-- App.tsx                         # Monta Redux, ThemeProvider, Core, Router y Toaster
|   `-- vite-env.d.ts                   # Tipos globales de Vite
|
|-- dist/                               # Build de producción
|-- package.json                        # Scripts y dependencias
|-- vite.config.ts                      # Configuración Vite, puerto 56221 y alias @ -> src
|-- tsconfig.json                       # Configuración TypeScript base
|-- tsconfig.app.json                   # Configuración TS de la aplicación
|-- tsconfig.node.json                  # Configuración TS para tooling Node
|-- components.json                     # Configuración shadcn/ui
|-- ARCHITECTURE.md                     # Documentación técnica ampliada
`-- README.md                           # Resumen del sistema
```

## Versión funcional y no tan detallada

Esta versión es la estructura recomendada para trabajar en el día a día. No baja tanto al detalle como la infraestructura completa, pero deja claro dónde guardar cada tipo de código.

```text
DOSAccessWeb/
|-- public/                         # Recursos públicos servidos por Vite
|
|-- src/
|   |-- config/                     # Configuración global: env, API, flags y app
|   |-- translations/               # i18n y textos por idioma
|   |-- assets/                     # Imágenes, iconos y estilos importados
|   |
|   |-- lib/                        # Código compartido global
|   |   |-- constants/              # Rutas, roles, permisos y storage keys
|   |   |-- utils/                  # Funciones puras y genéricas
|   |   |-- helpers/                # Lógica auxiliar con contexto de negocio
|   |   |-- hooks/                  # Hooks reutilizables globales
|   |   `-- types/                  # Tipos comunes de la aplicación
|   |
|   |-- components/                 # Componentes reutilizables
|   |   |-- core/                   # Inicialización de app, sesión y providers
|   |   |-- router/                 # Rutas públicas, privadas y guards
|   |   |-- layout/                 # Shell, sidebar, header y estructura base
|   |   |-- ui/                     # Botones, inputs, modales y componentes base
|   |   |-- common/                 # Componentes compartidos con sentido de negocio
|   |   `-- theme/                  # Tema visual y configuración de estilos
|   |
|   |-- pages/                      # Pantallas de la aplicación
|   |   |-- public/                 # Login, recuperación y páginas públicas
|   |   `-- private/                # Dashboard y módulos autenticados
|   |
|   |-- data/                       # Acceso a backend y estado global
|   |   |-- api/                    # Cliente HTTP, DTOs, mappers y services
|   |   `-- store/                  # Redux Toolkit: slices, thunks y selectors
|   |
|   |-- main.tsx                    # Entrada de React
|   |-- App.tsx                     # Providers, tema, router y toaster
|   `-- vite-env.d.ts               # Tipos globales de Vite
|
|-- dist/                           # Build de producción
|-- package.json                    # Scripts y dependencias
|-- vite.config.ts                  # Configuración de Vite
|-- tsconfig.json                   # Configuración TypeScript
|-- components.json                 # Configuración shadcn/ui
|-- ARCHITECTURE.md                 # Documentación técnica
`-- README.md                       # Resumen del proyecto
```

### Ejemplos mínimos de código que encajan en esta estructura

#### Configuración global

```text
src/config/api.config.ts
```

```ts
export const apiConfig = {
  baseUrl: import.meta.env.VITE_API_BASE_URL,
  timeout: 15000,
};
```

Esto debe ir en `config` porque define valores globales de infraestructura frontend.

#### Constantes

```text
src/lib/constants/routes.constants.ts
```

```ts
export const ROUTES = {
  login: "/login",
  dashboard: "/dashboard",
  locks: "/building/locks",
} as const;
```

Esto debe ir en `constants` porque son valores fijos reutilizados por varias partes de la app.

#### Cliente HTTP

```text
src/data/api/http/http-client.ts
```

```ts
import axios from "axios";
import { apiConfig } from "@/config/api.config";

export const httpClient = axios.create({
  baseURL: apiConfig.baseUrl,
  timeout: apiConfig.timeout,
});
```

Esto debe ir en `data/api` porque es infraestructura de comunicación con backend.

#### DTO

```text
src/data/api/modules/locks/locks.dto.ts
```

```ts
export type LockDto = {
  id: string;
  name: string;
  status: "online" | "offline";
};
```

Esto debe ir junto al módulo de API porque representa cómo llegan los datos desde backend.

#### Mapper

```text
src/data/api/modules/locks/locks.mapper.ts
```

```ts
import type { LockDto } from "./locks.dto";

export type Lock = {
  id: string;
  name: string;
  isOnline: boolean;
};

export function mapLockDtoToLock(dto: LockDto): Lock {
  return {
    id: dto.id,
    name: dto.name,
    isOnline: dto.status === "online",
  };
}
```

Esto debe ir en un `mapper` porque transforma datos del backend a un modelo más cómodo para el frontend.

#### Service

```text
src/data/api/modules/locks/locks.service.ts
```

```ts
import { httpClient } from "@/data/api/http/http-client";
import { mapLockDtoToLock } from "./locks.mapper";
import type { LockDto } from "./locks.dto";

export async function getLocks() {
  const response = await httpClient.get<LockDto[]>("/locks");
  return response.data.map(mapLockDtoToLock);
}
```

Esto debe ir en `service` porque llama al backend y devuelve datos preparados para la app.

#### Slice de Redux

```text
src/data/store/reducers/locks/locks.slice.ts
```

```ts
import { createSlice } from "@reduxjs/toolkit";
import { loadLocksThunk } from "./locks.thunks";
import type { Lock } from "@/data/api/modules/locks/locks.mapper";

type LocksState = {
  items: Lock[];
  loading: boolean;
};

const initialState: LocksState = {
  items: [],
  loading: false,
};

export const locksSlice = createSlice({
  name: "locks",
  initialState,
  reducers: {},
  extraReducers: (builder) => {
    builder
      .addCase(loadLocksThunk.pending, (state) => {
        state.loading = true;
      })
      .addCase(loadLocksThunk.fulfilled, (state, action) => {
        state.items = action.payload;
        state.loading = false;
      });
  },
});
```

Esto debe ir en `store` porque gestiona estado global.

#### Selector

```text
src/data/store/reducers/locks/locks.selectors.ts
```

```ts
import type { RootState } from "@/data/store";

export const selectLocks = (state: RootState) => state.locks.items;
export const selectLocksLoading = (state: RootState) => state.locks.loading;
```

Esto debe ir en `selectors` porque centraliza cómo se lee el estado.

#### Componente UI base

```text
src/components/ui/AppButton.tsx
```

```tsx
type AppButtonProps = {
  children: React.ReactNode;
  onClick?: () => void;
};

export function AppButton({ children, onClick }: AppButtonProps) {
  return (
    <button type="button" onClick={onClick}>
      {children}
    </button>
  );
}
```

Esto debe ir en `components/ui` porque es visual, reutilizable y no tiene negocio.

#### Componente privado de página

```text
src/pages/private/building/locks/components/LocksList.tsx
```

```tsx
import type { Lock } from "@/data/api/modules/locks/locks.mapper";

type LocksListProps = {
  locks: Lock[];
};

export function LocksList({ locks }: LocksListProps) {
  return (
    <ul>
      {locks.map((lock) => (
        <li key={lock.id}>{lock.name}</li>
      ))}
    </ul>
  );
}
```

Esto debe ir dentro de la página porque solo tiene sentido para la pantalla de cerraduras.

#### Guard de ruta privada

```text
src/components/router/PrivateRoute.tsx
```

```tsx
import { Navigate } from "react-router-dom";
import { ROUTES } from "@/lib/constants/routes.constants";
import { isAuthenticated } from "@/lib/helpers/auth.helpers";

type PrivateRouteProps = {
  children: React.ReactNode;
};

export function PrivateRoute({ children }: PrivateRouteProps) {
  if (!isAuthenticated()) {
    return <Navigate to={ROUTES.login} replace />;
  }

  return children;
}
```

Esto debe ir en `components/router` porque controla navegación y acceso.

