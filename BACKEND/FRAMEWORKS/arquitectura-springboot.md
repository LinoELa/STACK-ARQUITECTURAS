## Descripcion General

### Arquitectura backend con Spring Boot

Este ejemplo describe una posible arquitectura backend con Spring Boot para una cafeteria. La estructura sigue una organizacion tipica por capas y paquetes, pensada para mantener separadas las responsabilidades de negocio, persistencia e integracion.

## Infraestructura Tecnica

```text
cafeteria-service/
|-- src/
|   |-- main/
|   |   |-- java/com/cafeteria/
|   |   |   |-- CafeteriaApplication.java        # Punto de entrada de Spring Boot
|   |   |   |-- config/                          # Seguridad, CORS, Swagger y configuracion general
|   |   |   |   |-- SecurityConfig.java         # Configuracion de seguridad
|   |   |   |   `-- OpenApiConfig.java          # Configuracion de documentacion OpenAPI
|   |   |   |-- controller/                      # Endpoints REST del sistema
|   |   |   |   |-- MenuController.java         # API de carta y productos
|   |   |   |   `-- OrdersController.java       # API de pedidos
|   |   |   |-- service/                         # Logica de negocio principal
|   |   |   |   |-- MenuService.java            # Reglas de carta y categorias
|   |   |   |   `-- OrdersService.java          # Reglas de pedidos y estados
|   |   |   |-- repository/                      # Acceso a datos con JPA
|   |   |   |   |-- ProductRepository.java      # Acceso a productos
|   |   |   |   `-- OrderRepository.java        # Acceso a pedidos
|   |   |   |-- entity/                          # Entidades JPA del dominio
|   |   |   |   |-- ProductEntity.java          # Entidad de producto
|   |   |   |   `-- OrderEntity.java            # Entidad de pedido
|   |   |   |-- dto/                             # DTOs de entrada y salida
|   |   |   |   |-- CreateOrderRequest.java     # DTO de alta de pedido
|   |   |   |   `-- ProductResponse.java        # DTO de salida de producto
|   |   |   |-- domain/                          # Agrupacion funcional por negocio
|   |   |   |   |-- menu/                       # Carta, productos, combos y extras
|   |   |   |   `-- reservations/               # Reservas y gestion de mesas
|   |   |   |-- integration/                     # Pago, correo, delivery y TPV
|   |   |   |   |-- PaymentGatewayClient.java   # Cliente de pasarela de pago
|   |   |   |   `-- MailProviderClient.java     # Cliente de notificaciones por correo
|   |   |   |-- exception/                       # Manejo centralizado de errores
|   |   |   |   |-- GlobalExceptionHandler.java # Handler global de excepciones
|   |   |   |   `-- BusinessException.java      # Excepcion de negocio
|   |   |   |-- util/                            # Helpers y utilidades
|   |   |   |   |-- PriceUtils.java             # Formateo y calculo de importes
|   |   |   |   `-- DateUtils.java              # Utilidades de fechas y reservas
|   |   |   `-- security/                        # JWT, filtros y acceso
|   |   |       |-- JwtFilter.java              # Filtro JWT
|   |   |       `-- UserDetailsServiceImpl.java # Carga de usuarios para seguridad
|   |   `-- resources/
|   |       |-- application.yml                  # Configuracion principal
|   |       `-- data.sql                         # Datos semilla si aplica
|   `-- test/
|       `-- java/com/cafeteria/                  # Tests unitarios e integracion
|-- pom.xml                                      # Dependencias y build Maven
|-- mvnw                                         # Wrapper Maven
|-- mvnw.cmd                                     # Wrapper Maven para Windows
|-- README.md                                    # Resumen del proyecto y arranque
`-- ARCHITECTURE.md                              # Documentacion tecnica ampliada
```

## Arquitectura Funcional

```text
cafeteria-service
|-- Capa de entrada
|   |-- Spring Boot Application            # Arranque del servicio
|   `-- REST Controllers                   # Endpoints HTTP del sistema
|-- Capa transversal
|   `-- Seguridad y observabilidad         # JWT, validacion, excepciones y logging
|-- Capa de aplicacion
|   |-- Casos de uso operativos            # Carta, reservas, pedidos, cocina y pagos
|   `-- Casos de uso administrativos       # Inventario, personal y reportes
|-- Capa de dominio
|   |-- Atencion al cliente                # Cliente, mesa, reserva y pedido
|   |-- Operacion del local                # Cocina, estados, turnos y entrega
|   `-- Gestion comercial                  # Productos, stock, caja y promociones
|-- Capa de persistencia
|   `-- JPA Repositories                   # Acceso a datos relacional
|-- Capa de integracion
|   |-- Pago                               # Cobros y confirmaciones
|   |-- Correo                             # Avisos y confirmaciones
|   |-- Delivery                           # Envio externo de pedidos
|   `-- TPV                                # Punto de venta y tickets
`-- Infraestructura externa
    `-- Base de datos                      # Persistencia principal del negocio
```

## Flujo Principal

```text
Cliente o personal del local
  -> Request HTTP
  -> Filtros de seguridad
  -> Controller
  -> Service
  -> Repository o cliente externo
  -> Base de datos / servicio externo
  -> Response HTTP
  -> Exception Handler si algo falla
```

## Patron Interno De Un Modulo

```text
module-name/
|-- controller/                           # Endpoints REST del modulo
|-- service/                              # Reglas de negocio
|-- repository/                           # Acceso a datos
|-- entity/                               # Entidades JPA
|-- dto/                                  # Contratos de entrada y salida
`-- integration/                          # Adaptadores externos si aplica
```

## Ejemplo Logico Del Dominio

```text
Contexto comercial
Carta
  -> Categoria
  -> Producto
  -> Extra
  -> Promocion

Contexto de sala
Cliente
  -> Reserva
  -> Mesa
  -> Pedido
  -> Pago

Contexto operativo
Pedido
  -> Cocina
  -> Estado
  -> Ticket
  -> Entrega

Contexto de gestion
Admin
  -> Inventario
  -> Personal
  -> Caja
  -> Reportes
```
