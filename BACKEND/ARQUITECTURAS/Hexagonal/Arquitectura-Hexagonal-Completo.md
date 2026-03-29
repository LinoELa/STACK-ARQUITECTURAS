# Arquitectura Hexagonal + Vertical Slicing + Screaming (Version Completa)

Esta version completa muestra una arquitectura mas detallada para el mismo ejemplo de `backend-reference-express`.

El objetivo aqui no es solo ver la idea general, sino ver como podria quedar un backend real cuando el proyecto ya necesita una separacion mas fina por responsabilidad.

Los slices principales siguen siendo:

- `auth`
- `movies`
- `watchlist`

---

## 1. Idea central

La arquitectura debe seguir gritando negocio, pero con una estructura mas preparada para crecer.

La diferencia frente a la version minimalista es que aqui:

- se separan mejor controllers, persistence, validation y security
- los casos de uso viven por accion
- cada modulo queda listo para crecer sin mezclar responsabilidades

La regla sigue siendo la misma:

- el dominio vive en el centro
- la aplicacion orquesta
- la infraestructura adapta

---

## 2. Estructura completa recomendada

```text
backend-reference-express/
|-- server.js                                 # Punto de entrada del backend
|-- package.json                              # Dependencias y scripts
|-- README.md                                 # Guia general del proyecto
|
|-- docs/
|   `-- architecture/
|       |-- Arquitectura-Hexagonal-Minimalista.md # Version ligera de la guia
|       `-- Arquitectura-Hexagonal-Completo.md    # Version detallada de la guia
|
|-- src/
|   |-- app/
|   |   |-- createApp.js                      # Configura Express y middlewares globales
|   |   |-- registerRoutes.js                 # Registra rutas de todos los modulos
|   |   `-- buildModules.js                   # Construye contenedores por modulo
|   |
|   |-- shared/
|   |   |-- domain/
|   |   |   `-- errors/
|   |   |       `-- AppError.js               # Error base del dominio compartido
|   |   `-- infrastructure/
|   |       |-- prisma/
|   |       |   `-- prismaClient.js           # Cliente Prisma compartido
|   |       `-- http/
|   |           `-- middlewares/
|   |               |-- authMiddleware.js     # Proteccion de rutas autenticadas
|   |               |-- validateRequest.js    # Validacion comun de payloads
|   |               |-- errorHandler.js       # Manejo global de errores
|   |               `-- notFoundHandler.js    # Respuesta para rutas inexistentes
|   |
|   `-- modules/
|       |-- auth/
|       |   |-- domain/
|       |   |   |-- entities/
|       |   |   |   `-- User.js               # Entidad usuario
|       |   |   `-- ports/
|       |   |       |-- AuthRepository.js     # Contrato de persistencia de usuarios
|       |   |       |-- PasswordHasher.js     # Contrato de hash y comparacion
|       |   |       `-- TokenService.js       # Contrato de generacion de tokens
|       |   |-- application/
|       |   |   |-- register/
|       |   |   |   `-- RegisterUserUseCase.js # Caso de uso de registro
|       |   |   |-- login/
|       |   |   |   `-- LoginUserUseCase.js   # Caso de uso de login
|       |   |   `-- logout/
|       |   |       `-- LogoutUseCase.js      # Caso de uso de logout
|       |   `-- infrastructure/
|       |       |-- persistence/
|       |       |   `-- PrismaAuthRepository.js # Repositorio Prisma del modulo
|       |       |-- security/
|       |       |   |-- BcryptPasswordHasher.js # Implementacion concreta del hasher
|       |       |   `-- JwtTokenService.js    # Implementacion concreta de tokens
|       |       |-- validation/
|       |       |   `-- authSchemas.js        # Esquemas de validacion de auth
|       |       |-- http/
|       |       |   |-- authRoutes.js         # Rutas HTTP del modulo
|       |       |   `-- controllers/
|       |       |       |-- RegisterPostController.js # Controller de registro
|       |       |       |-- LoginPostController.js    # Controller de login
|       |       |       `-- LogoutPostController.js   # Controller de logout
|       |       `-- container.js              # Wiring del modulo auth
|       |
|       |-- movies/
|       |   |-- domain/
|       |   |   |-- entities/
|       |   |   |   `-- Movie.js              # Entidad pelicula
|       |   |   `-- ports/
|       |   |       `-- MovieRepository.js    # Contrato de persistencia de peliculas
|       |   |-- application/
|       |   |   |-- create/
|       |   |   |   `-- CreateMovieUseCase.js # Caso de uso para crear peliculas
|       |   |   |-- list/
|       |   |   |   `-- ListMoviesUseCase.js  # Caso de uso para listar peliculas
|       |   |   |-- update/
|       |   |   |   `-- UpdateMovieUseCase.js # Caso de uso para editar peliculas
|       |   |   `-- remove/
|       |   |       `-- RemoveMovieUseCase.js # Caso de uso para eliminar peliculas
|       |   `-- infrastructure/
|       |       |-- persistence/
|       |       |   `-- PrismaMovieRepository.js # Repositorio Prisma del modulo
|       |       |-- validation/
|       |       |   `-- movieSchemas.js       # Validaciones del modulo movies
|       |       |-- http/
|       |       |   |-- movieRoutes.js        # Rutas HTTP del modulo
|       |       |   `-- controllers/
|       |       |       |-- CreateMoviePostController.js  # Controller de alta
|       |       |       |-- ListMoviesGetController.js    # Controller de listado
|       |       |       |-- UpdateMoviePutController.js   # Controller de actualizacion
|       |       |       `-- RemoveMovieDeleteController.js # Controller de borrado
|       |       `-- container.js              # Wiring del modulo movies
|       |
|       `-- watchlist/
|           |-- domain/
|           |   |-- entities/
|           |   |   `-- WatchlistItem.js      # Entidad de item de watchlist
|           |   `-- ports/
|           |       `-- WatchlistRepository.js # Contrato de persistencia
|           |-- application/
|           |   |-- add/
|           |   |   `-- AddMovieToWatchlistUseCase.js   # Agregar pelicula
|           |   |-- list/
|           |   |   `-- ListWatchlistUseCase.js         # Listar watchlist
|           |   |-- update/
|           |   |   `-- UpdateWatchlistItemUseCase.js   # Editar item
|           |   `-- remove/
|           |       `-- RemoveWatchlistItemUseCase.js   # Eliminar item
|           `-- infrastructure/
|               |-- persistence/
|               |   `-- PrismaWatchlistRepository.js    # Repositorio Prisma del modulo
|               |-- validation/
|               |   `-- watchlistSchemas.js             # Validaciones del modulo
|               |-- http/
|               |   |-- watchlistRoutes.js              # Rutas HTTP de watchlist
|               |   `-- controllers/
|               |       |-- AddWatchlistPostController.js    # Controller de alta
|               |       |-- ListWatchlistGetController.js    # Controller de listado
|               |       |-- UpdateWatchlistPutController.js  # Controller de actualizacion
|               |       `-- RemoveWatchlistDeleteController.js # Controller de borrado
|               `-- container.js              # Wiring del modulo watchlist
|
|-- prisma/
|   |-- schema.prisma                         # Definicion del modelo Prisma
|   |-- migrations/                          # Migraciones versionadas
|   `-- seed/
|       |-- index.js                         # Entrada principal del seed
|       |-- userSeed.js                      # Datos iniciales de usuarios
|       |-- movieSeed.js                     # Datos iniciales de peliculas
|       `-- watchlistSeed.js                 # Datos iniciales de watchlist
|
`-- tests/
    |-- unit/
    |   |-- auth/
    |   |   |-- RegisterUserUseCase.test.js  # Test unitario de registro
    |   |   `-- LoginUserUseCase.test.js     # Test unitario de login
    |   |-- movies/
    |   |   `-- CreateMovieUseCase.test.js   # Test unitario de alta de peliculas
    |   `-- watchlist/
    |       `-- AddMovieToWatchlistUseCase.test.js # Test unitario de watchlist
    |
    `-- integration/
        |-- auth/
        |   `-- authRoutes.test.js           # Pruebas de integracion HTTP
        |-- movies/
        |   `-- movieRoutes.test.js          # Pruebas de integracion HTTP
        `-- watchlist/
            `-- watchlistRoutes.test.js      # Pruebas de integracion HTTP
```

---

## 3. Como leer esta version

En la version completa, cada modulo mantiene la base:

```text
feature/
|-- domain/
|-- application/
`-- infrastructure/
```

Pero cada una de esas capas ya esta mas refinada.

### `domain`

Aqui viven los conceptos puros del negocio.

Ejemplos:

- `User.js`
- `Movie.js`
- `WatchlistItem.js`
- `AuthRepository.js`
- `MovieRepository.js`
- `WatchlistRepository.js`

Aqui no deben entrar:

- controllers
- rutas
- Prisma
- middlewares
- validaciones HTTP

### `application`

Aqui viven los casos de uso por accion concreta.

Ejemplos:

- `register/RegisterUserUseCase.js`
- `login/LoginUserUseCase.js`
- `create/CreateMovieUseCase.js`
- `remove/RemoveWatchlistItemUseCase.js`

Esta capa:

- orquesta el flujo del modulo
- usa puertos del dominio
- decide que debe ocurrir
- no conoce detalles de framework

### `infrastructure`

Aqui viven las implementaciones concretas y adaptadores.

Ejemplos:

- `persistence/PrismaAuthRepository.js`
- `security/JwtTokenService.js`
- `validation/movieSchemas.js`
- `http/controllers/CreateMoviePostController.js`
- `container.js`

---

## 4. Regla de dependencias

La regla sigue siendo:

```text
infrastructure -> application -> domain
```

Eso significa:

- `domain` no depende de Express
- `domain` no depende de Prisma
- `application` depende de contratos del dominio
- `infrastructure` implementa esos contratos

Un recorrido normal queda asi:

```text
Route
  -> Controller
  -> Use Case
  -> Domain Port
  -> Prisma Repository / JWT / bcrypt
  -> Response
```

---

## 5. Donde van Prisma, migrations y seed

En este ejemplo, Prisma sigue viviendo fuera de `src/` porque es infraestructura global.

Por eso tiene sentido dejar:

- `prisma/schema.prisma`
- `prisma/migrations/`
- `prisma/seed/index.js`
- `prisma/seed/userSeed.js`
- `prisma/seed/movieSeed.js`
- `prisma/seed/watchlistSeed.js`

La regla importante es esta:

- la persistencia concreta vive fuera como tecnologia global
- cada modulo sigue teniendo su propio repositorio concreto dentro de `infrastructure/persistence`

---

## 6. Cuando usar esta version

Usa esta version cuando:

- el proyecto ya crecio y quieres mas orden por responsabilidad
- quieres separar mejor HTTP, validation, persistence y security
- necesitas controllers por endpoint
- quieres tests unitarios y de integracion mas claros por modulo
- quieres una arquitectura lista para seguir escalando

---

## 7. Resumen rapido

La version completa se resume asi:

- mantiene los mismos slices `auth`, `movies` y `watchlist`
- separa mejor los detalles tecnicos dentro de `infrastructure`
- deja los casos de uso organizados por accion
- conserva Prisma, migrations y seed como infraestructura global

En corto:

```text
Hexagonal = protege el dominio
Vertical Slicing = organiza todo por feature
Screaming = el proyecto grita negocio desde la raiz
```
