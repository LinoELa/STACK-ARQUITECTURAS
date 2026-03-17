## Descripcion General

### Arquitectura movil con React Native

Este ejemplo describe una posible arquitectura movil con React Native para una cafeteria. La aplicacion se organiza alrededor de navegacion, pantallas, estado global y servicios, permitiendo ofrecer una experiencia clara para clientes o personal del local.

## Infraestructura Tecnica

```text
cafeteria-mobile/
|-- App.tsx                               # Punto de entrada de la aplicacion React Native
|-- app.json                              # Configuracion general de la app movil
|-- babel.config.js                       # Alias, plugins y transformacion del proyecto
|-- package.json                          # Scripts y dependencias de la aplicacion
|-- assets/                               # Iconos, imagenes, fuentes y recursos estaticos
|-- src/
|   |-- navigation/                       # Navegacion principal de la app
|   |   |-- root-navigator.tsx            # Navegador principal de la aplicacion
|   |   `-- auth-navigator.tsx            # Flujo de login y acceso
|   |-- screens/                          # Pantallas principales del sistema
|   |   |-- menu/menu-screen.tsx          # Pantalla de carta y productos
|   |   `-- reservations/reservations-screen.tsx # Pantalla de reservas
|   |-- components/                       # Componentes reutilizables de interfaz
|   |   |-- ui/button.tsx                 # Boton base del sistema
|   |   `-- ui/input.tsx                  # Campo de entrada reutilizable
|   |-- features/                         # Modulos funcionales por dominio
|   |   |-- cart/cart-sheet.tsx           # Vista rapida del carrito
|   |   `-- orders/order-status-card.tsx  # Estado visual del pedido
|   |-- services/                         # Cliente API y acceso a backend
|   |   |-- menu.service.ts               # Consumo de carta, categorias y productos
|   |   `-- orders.service.ts             # Consumo de pedidos y seguimiento
|   |-- store/                            # Estado global de la aplicacion
|   |   |-- cart.store.ts                 # Estado del carrito
|   |   `-- session.store.ts              # Estado de sesion del usuario
|   |-- hooks/                            # Hooks personalizados
|   |   |-- use-cart.ts                   # Acciones y lectura del carrito
|   |   `-- use-session.ts                # Estado y acciones de sesion
|   |-- lib/                              # Helpers, constantes y utilidades
|   |   |-- format-price.ts               # Formateo de importes
|   |   `-- calculate-order-total.ts      # Calculo de total del pedido
|   |-- theme/                            # Tokens visuales y estilos base
|   |   |-- colors.ts                     # Paleta de colores
|   |   `-- spacing.ts                    # Espaciados y medidas
|   |-- types/                            # Tipos compartidos del proyecto
|   |   |-- product.types.ts              # Tipos de carta y producto
|   |   `-- order.types.ts                # Tipos de pedido
|   `-- providers/                        # Providers globales de la app
|       |-- session-provider.tsx          # Contexto o provider de sesion
|       `-- theme-provider.tsx            # Contexto o provider visual
|-- .env                                  # Variables de entorno locales
|-- README.md                             # Resumen del proyecto y arranque
`-- ARCHITECTURE.md                       # Documentacion tecnica ampliada
```

## Arquitectura Funcional

```text
cafeteria-mobile
|-- Capa de presentacion
|   |-- screens/                          # Pantallas navegables de la app
|   `-- components/                       # Componentes visuales reutilizables
|-- Capa de navegacion
|   `-- navigation/                       # Flujo entre login, menu, pedidos y reservas
|-- Capa de dominio de interfaz
|   `-- features/                         # Carrito, pedidos, reservas y estados visuales
|-- Capa de estado
|   `-- store/                            # Carrito, sesion, preferencias y datos temporales
|-- Capa de integracion
|   `-- services/                         # Cliente HTTP hacia el backend de cafeteria
|-- Capa transversal
|   |-- hooks/                            # Logica compartida entre pantallas
|   |-- providers/                        # Providers globales
|   |-- lib/                              # Utilidades y helpers
|   `-- theme/                            # Sistema visual de la aplicacion
`-- Backend consumido
    `-- CafeteriaService                  # API REST para carta, reservas, pedidos y pagos
```

## Flujo Principal

```text
Usuario movil
  -> Navega entre pantallas
  -> Interactua con components/ y features/
  -> Dispara hooks o acciones del store
  -> Consulta services/
  -> Recibe datos del backend
  -> Actualiza store/ o estado local
  -> Re-renderiza la interfaz
  -> Muestra feedback visual o notificaciones
```

## Patron Interno De Un Modulo

```text
feature-name/
|-- screen/                              # Pantalla principal del modulo
|-- components/                          # Componentes visuales del modulo
|-- hooks/                               # Logica de cliente del modulo
|-- service/                             # Acceso a datos del modulo
`-- types/                               # Tipos propios del modulo
```

## Ejemplo Logico Del Dominio

```text
Contexto comercial
Carta
  -> Categoria
  -> Producto
  -> Extra
  -> Promocion

Contexto de cliente
Usuario
  -> Reserva
  -> Carrito
  -> Pedido
  -> Pago

Contexto operativo
Pedido
  -> Cocina
  -> Estado
  -> Ticket
  -> Entrega

Contexto de gestion
Admin o staff
  -> Menu
  -> Reservas
  -> Inventario
  -> Reportes
```
