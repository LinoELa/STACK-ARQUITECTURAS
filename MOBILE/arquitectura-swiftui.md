## Descripcion General

### Arquitectura movil nativa con SwiftUI

Este ejemplo describe una posible arquitectura movil nativa con SwiftUI para una cafeteria. La organizacion en views, viewmodels, networking y almacenamiento local ayuda a construir una app clara, mantenible y alineada con el ecosistema iOS.

## Infraestructura Tecnica

```text
CafeteriaApp/
|-- CafeteriaApp.swift                    # Punto de entrada principal de la app SwiftUI
|-- App/
|   |-- RootView.swift                    # Vista raiz y coordinacion inicial
|   `-- AppRouter.swift                   # Navegacion principal de la aplicacion
|-- Features/
|   |-- Menu/
|   |   |-- Views/
|   |   |   |-- MenuView.swift            # Pantalla de carta y productos
|   |   |   `-- ProductCardView.swift     # Tarjeta visual de producto
|   |   |-- ViewModels/
|   |   |   `-- MenuViewModel.swift       # Estado y logica de la carta
|   |   `-- Models/
|   |       `-- MenuItem.swift            # Modelo de producto de cafeteria
|   |-- Reservations/
|   |   |-- Views/
|   |   |   `-- ReservationsView.swift    # Pantalla de reservas
|   |   `-- ViewModels/
|   |       `-- ReservationsViewModel.swift # Estado y logica de reservas
|   |-- Orders/
|   |   |-- Views/
|   |   |   `-- OrdersView.swift          # Seguimiento de pedidos
|   |   `-- ViewModels/
|   |       `-- OrdersViewModel.swift     # Estado y logica de pedidos
|   `-- Cart/
|       |-- Views/
|       |   `-- CartView.swift            # Vista del carrito
|       `-- ViewModels/
|           `-- CartViewModel.swift       # Estado y acciones del carrito
|-- Core/
|   |-- Networking/
|   |   |-- APIClient.swift               # Cliente HTTP hacia el backend
|   |   `-- Endpoints.swift               # Definicion de endpoints
|   |-- Storage/
|   |   |-- SessionStore.swift            # Persistencia local de sesion
|   |   `-- CartStore.swift               # Persistencia local del carrito
|   |-- DesignSystem/
|   |   |-- AppColors.swift               # Colores y tokens visuales
|   |   `-- AppTypography.swift           # Tipografia y estilos base
|   `-- Utils/
|       |-- PriceFormatter.swift          # Formateo de importes
|       `-- DateFormatterHelper.swift     # Utilidades de fecha y hora
|-- Shared/
|   |-- Components/
|   |   |-- PrimaryButton.swift           # Boton principal reutilizable
|   |   `-- LoadingView.swift             # Componente de carga
|   `-- Models/
|       |-- Order.swift                   # Modelo compartido de pedido
|       `-- Customer.swift                # Modelo compartido de cliente
|-- Resources/
|   |-- Assets.xcassets                   # Imagenes, iconos y colores
|   `-- Localizable.strings               # Textos localizados
|-- Tests/
|   |-- UnitTests/                        # Tests unitarios
|   `-- UITests/                          # Tests de interfaz
|-- README.md                             # Resumen del proyecto y arranque
`-- ARCHITECTURE.md                       # Documentacion tecnica ampliada
```

## Arquitectura Funcional

```text
CafeteriaApp
|-- Capa de presentacion
|   |-- Views SwiftUI                     # Pantallas y componentes visuales
|   `-- Design System                     # Botones, colores, tipografia y estados visuales
|-- Capa de estado
|   `-- ViewModels                        # Logica de interfaz y estado observable
|-- Capa de dominio de interfaz
|   `-- Features                          # Menu, reservas, carrito, pedidos y cuenta
|-- Capa de integracion
|   `-- Networking                        # Cliente HTTP y endpoints del backend
|-- Capa de persistencia local
|   `-- Storage                           # Sesion, carrito y preferencias del usuario
|-- Capa transversal
|   |-- Shared Components                 # Componentes reutilizables
|   |-- Shared Models                     # Modelos comunes
|   `-- Utils                             # Helpers y utilidades
`-- Backend consumido
    `-- CafeteriaService                  # API REST para carta, reservas, pedidos y pagos
```

## Flujo Principal

```text
Usuario iPhone/iPad
  -> Navega por Views SwiftUI
  -> Interactua con componentes
  -> Dispara acciones del ViewModel
  -> Consulta APIClient o Storage local
  -> Recibe datos del backend
  -> Actualiza estado observable
  -> Re-renderiza la interfaz
  -> Muestra feedback visual
```

## Patron Interno De Un Modulo

```text
FeatureName/
|-- Views/                               # Pantallas y componentes del modulo
|-- ViewModels/                          # Estado y logica del modulo
|-- Models/                              # Modelos propios del modulo
`-- Services/                            # Servicios especificos si el modulo los necesita
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
