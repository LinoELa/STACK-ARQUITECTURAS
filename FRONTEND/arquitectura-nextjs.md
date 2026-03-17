## Descripcion General

### Arquitectura web con Next.js

Este ejemplo describe una posible arquitectura frontend con Next.js para una cafeteria. La propuesta combina App Router, componentes reutilizables, logica por features y consumo de servicios para construir una experiencia web moderna y ordenada.

## Infraestructura Tecnica

```text
cafeteria-web/
|-- src/
|   |-- app/                              # App Router de Next.js
|   |   |-- layout.tsx                    # Layout raiz de la aplicacion
|   |   |-- page.tsx                      # Landing principal de la cafeteria
|   |   |-- menu/page.tsx                 # Vista de carta y productos
|   |   |-- reservations/page.tsx         # Vista de reservas
|   |   |-- orders/page.tsx               # Seguimiento de pedidos del cliente
|   |   |-- admin/page.tsx                # Panel interno del negocio
|   |   |-- api/menu/route.ts             # Endpoint interno para menu
|   |   `-- api/orders/route.ts           # Endpoint interno para pedidos
|   |-- components/                       # Componentes reutilizables de interfaz
|   |   |-- ui/button.tsx                 # Boton base del sistema
|   |   `-- ui/modal.tsx                  # Modal reutilizable
|   |-- features/                         # Componentes y logica por dominio
|   |   |-- menu/product-card.tsx         # Tarjeta de producto de la carta
|   |   `-- reservations/reservation-form.tsx # Formulario de reserva
|   |-- services/                         # Cliente API y consumo de servicios externos
|   |   |-- menu.service.ts               # Peticiones de carta y categorias
|   |   `-- orders.service.ts             # Peticiones de pedidos y estados
|   |-- actions/                          # Server Actions de Next.js
|   |   |-- create-reservation.action.ts  # Accion para crear reservas
|   |   `-- create-order.action.ts        # Accion para lanzar pedidos
|   |-- hooks/                            # Hooks personalizados del frontend
|   |   |-- use-cart.ts                   # Gestion del carrito
|   |   `-- use-session.ts                # Gestion de sesion del usuario
|   |-- store/                            # Estado global del cliente
|   |   |-- cart.store.ts                 # Estado del carrito
|   |   `-- ui.store.ts                   # Estado de modales, filtros o drawers
|   |-- lib/                              # Helpers, constantes y utilidades
|   |   |-- format-price.ts               # Formateo de precios
|   |   `-- calculate-order-total.ts      # Calculo de totales del pedido
|   |-- types/                            # Tipos compartidos del proyecto
|   |   |-- product.types.ts              # Tipos de producto y carta
|   |   `-- order.types.ts                # Tipos de pedido
|   |-- styles/                           # Estilos globales y tokens visuales
|   |   |-- globals.css                   # Estilos globales
|   |   `-- theme.css                     # Variables visuales de la cafeteria
|   `-- middleware.ts                     # Proteccion de rutas y control de acceso
|-- public/                               # Imagenes, iconos y recursos publicos
|-- .env.local                            # Variables locales
|-- package.json                          # Scripts y dependencias del proyecto
|-- next.config.js                        # Configuracion de Next.js
|-- tsconfig.json                         # Configuracion TypeScript
|-- README.md                             # Resumen del proyecto y arranque
`-- ARCHITECTURE.md                       # Documentacion tecnica ampliada
```

## Arquitectura Funcional

```text
cafeteria-web
|-- Capa de presentacion
|   |-- app/                              # Paginas renderizadas con App Router
|   `-- components/                       # Componentes visuales reutilizables
|-- Capa de dominio de interfaz
|   `-- features/                         # Menu, reservas, pedidos y panel admin
|-- Capa de interaccion
|   |-- hooks/                            # Logica de interfaz y experiencia de usuario
|   `-- actions/                          # Mutaciones del lado servidor con Server Actions
|-- Capa de estado
|   `-- store/                            # Carrito, sesion y estado UI
|-- Capa de integracion
|   |-- services/                         # Cliente HTTP hacia backend o API interna
|   `-- app/api/                          # Endpoints internos de Next.js cuando aplica
|-- Capa transversal
|   |-- lib/                              # Helpers y utilidades compartidas
|   |-- types/                            # Tipado comun
|   `-- middleware.ts                     # Control de acceso y reglas globales
`-- Backend consumido
    `-- CafeteriaService                  # API REST para carta, reservas, pedidos y pagos
```

## Flujo Principal

```text
Cliente
  -> Navega por una ruta de app/
  -> Interactua con components/ y features/
  -> Dispara hooks o Server Actions
  -> Consulta services/ o app/api/
  -> Recibe datos del backend
  -> Actualiza store/ o estado local
  -> Re-renderiza la interfaz
  -> Muestra feedback visual
```

## Patron Interno De Un Modulo

```text
feature-name/
|-- components/                          # Piezas visuales del modulo
|-- hooks/                               # Logica de cliente del modulo
|-- actions/                             # Mutaciones del modulo
|-- services/                            # Acceso a datos del modulo
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
Admin
  -> Menu
  -> Reservas
  -> Inventario
  -> Reportes
```
