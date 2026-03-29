# Arquitectura Hexagonal + Vertical Slicing + Screaming (Version Minimalista)

Esta version minimalista muestra una forma simple y realista de organizar un backend de referencia.

El ejemplo esta pensado sobre un proyecto tipo `backend-reference-express` con tres slices principales:

- `auth`
- `movies`
- `watchlist`

La idea no es meter demasiadas carpetas, sino dejar una base clara, facil de leer y facil de crecer.

---

## 1. Idea central

La estructura debe gritar negocio antes que tecnologia.

Cuando alguien abra `src/`, deberia entender rapido que el sistema trata de:

- autenticacion
- peliculas
- watchlist

y no ver primero carpetas globales como:

- `controllers`
- `routes`
- `services`
- `repositories`

La regla base es esta:

- cada feature vive junta
- el dominio no depende de Express ni Prisma
- la infraestructura implementa lo que el dominio necesita

---

## 2. Estructura minimalista recomendada

```text
backend-reference-express/
|-- server.js                                 # Punto de entrada del backend
|-- package.json                              # Scripts y dependencias
|-- README.md                                 # Guia general del proyecto
|
|-- src/
|   |-- app/
|   |   |-- createApp.js                      # Configura la aplicacion web
|   |   `-- registerRoutes.js                 # Monta auth, movies y watchlist
|   |
|   |-- shared/
|   |   |-- errors/
|   |   |   `-- AppError.js                   # Error reutilizable para toda la app
|   |   |-- middlewares/
|   |   |   |-- authMiddleware.js             # Middleware de autenticacion
|   |   |   |-- validateRequest.js            # Validacion de entrada
|   |   |   `-- errorHandler.js               # Manejo centralizado de errores
|   |   `-- prisma/
|   |       `-- prismaClient.js               # Cliente Prisma compartido
|   |
|   `-- modules/
|       |-- auth/
|       |   |-- domain/
|       |   |   |-- User.js                   # Entidad principal del modulo
|       |   |   |-- AuthRepository.js         # Puerto para persistencia de usuarios
|       |   |   |-- PasswordHasher.js         # Puerto para hash y comparacion
|       |   |   `-- TokenService.js           # Puerto para JWT o tokens
|       |   |-- application/
|       |   |   |-- RegisterUserUseCase.js    # Caso de uso para registro
|       |   |   |-- LoginUserUseCase.js       # Caso de uso para login
|       |   |   `-- LogoutUseCase.js          # Caso de uso para logout
|       |   `-- infrastructure/
|       |       |-- authRoutes.js             # Rutas HTTP del modulo
|       |       |-- authSchemas.js            # Esquemas de validacion
|       |       |-- authControllers.js        # Adaptadores HTTP
|       |       |-- PrismaAuthRepository.js   # Implementacion del repositorio
|       |       |-- BcryptPasswordHasher.js   # Implementacion del hasher
|       |       |-- JwtTokenService.js        # Implementacion de tokens
|       |       `-- container.js              # Wiring del modulo
|       |
|       |-- movies/
|       |   |-- domain/
|       |   |   |-- Movie.js                  # Entidad principal de peliculas
|       |   |   `-- MovieRepository.js        # Puerto de persistencia
|       |   |-- application/
|       |   |   |-- CreateMovieUseCase.js     # Crear pelicula
|       |   |   |-- ListMoviesUseCase.js      # Listar peliculas
|       |   |   |-- UpdateMovieUseCase.js     # Editar pelicula
|       |   |   `-- RemoveMovieUseCase.js     # Eliminar pelicula
|       |   `-- infrastructure/
|       |       |-- movieRoutes.js            # Rutas HTTP del modulo
|       |       |-- movieSchemas.js           # Validaciones del modulo
|       |       |-- movieControllers.js       # Controllers HTTP
|       |       |-- PrismaMovieRepository.js  # Repositorio concreto
|       |       `-- container.js              # Wiring del modulo
|       |
|       `-- watchlist/
|           |-- domain/
|           |   |-- WatchlistItem.js          # Entidad principal de watchlist
|           |   `-- WatchlistRepository.js    # Puerto de acceso a datos
|           |-- application/
|           |   |-- AddMovieToWatchlistUseCase.js    # Agregar pelicula
|           |   |-- ListWatchlistUseCase.js          # Listar watchlist
|           |   |-- UpdateWatchlistItemUseCase.js    # Editar item
|           |   `-- RemoveWatchlistItemUseCase.js    # Eliminar item
|           `-- infrastructure/
|               |-- watchlistRoutes.js        # Rutas del modulo
|               |-- watchlistSchemas.js       # Validaciones del modulo
|               |-- watchlistControllers.js   # Controllers del modulo
|               |-- PrismaWatchlistRepository.js # Repositorio concreto
|               `-- container.js              # Wiring del modulo
|
|-- prisma/
|   |-- schema.prisma                         # Modelo de datos de Prisma
|   |-- migrations/                          # Historial de migraciones
|   `-- seed/
|       |-- index.js                         # Punto de entrada del seed
|       |-- userSeed.js                      # Seed de usuarios
|       |-- movieSeed.js                     # Seed de peliculas
|       `-- watchlistSeed.js                 # Seed de watchlist
|
`-- tests/
    |-- auth/                                # Pruebas del modulo auth
    |-- movies/                              # Pruebas del modulo movies
    `-- watchlist/                           # Pruebas del modulo watchlist
```

---

## 3. Como leer cada modulo

En esta version minimalista, cada slice sigue esta forma:

```text
feature/
|-- domain/                                  # Reglas, entidades y contratos
|-- application/                             # Casos de uso
`-- infrastructure/                          # HTTP, Prisma, seguridad y wiring
```

### `domain`

Aqui viven las reglas del negocio y los contratos del modulo.

Ejemplos del modulo `auth`:

- `User.js`
- `AuthRepository.js`
- `PasswordHasher.js`
- `TokenService.js`

Aqui no deben vivir:

- rutas
- controllers
- Prisma
- JWT
- bcrypt

### `application`

Aqui viven los casos de uso.

Ejemplos:

- `RegisterUserUseCase.js`
- `LoginUserUseCase.js`
- `CreateMovieUseCase.js`
- `AddMovieToWatchlistUseCase.js`

La capa de aplicacion:

- orquesta el flujo
- usa puertos del dominio
- no accede directo a detalles tecnicos

### `infrastructure`

Aqui viven las implementaciones concretas.

Ejemplos:

- `authRoutes.js`
- `movieControllers.js`
- `PrismaMovieRepository.js`
- `JwtTokenService.js`
- `container.js`

---

## 4. Regla de dependencias

La regla correcta es esta:

```text
infrastructure -> application -> domain
```

Eso significa:

- `domain` no conoce Express
- `domain` no conoce Prisma
- `application` usa contratos del dominio
- `infrastructure` implementa esos contratos

---

## 5. Flujo habitual

En esta estructura, un flujo normal suele quedar asi:

```text
Route
  -> Controller
  -> Use Case
  -> Domain Port
  -> Prisma Repository o servicio tecnico
  -> Response
```

---

## 6. Donde entra Prisma y seed

`prisma/` queda fuera de `src/` porque sigue siendo infraestructura global del proyecto.

Aqui tiene sentido dejar:

- `schema.prisma`
- `migrations/`
- `seed/index.js`
- seeds por modulo como `userSeed.js`, `movieSeed.js` y `watchlistSeed.js`

---

## 7. Cuando usar esta version

Usa esta version cuando:

- el proyecto todavia es pequeno o mediano
- tienes pocos modulos funcionales
- quieres Hexagonal real sin sobrecargar carpetas
- Prisma es la persistencia principal
- quieres que el repo se siga leyendo facil

---

## 8. Resumen rapido

La version minimalista se resume asi:

- `auth`, `movies` y `watchlist` son los slices principales
- cada slice tiene `domain`, `application` e `infrastructure`
- la tecnologia queda en los bordes
- Prisma y seed viven como infraestructura global

En corto:

```text
Hexagonal = separa negocio de tecnologia
Vertical Slicing = agrupa todo por feature
Screaming = el repo grita auth, movies y watchlist
```
