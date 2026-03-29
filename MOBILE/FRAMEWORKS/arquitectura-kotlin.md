## Descripcion General

### Arquitectura movil con Kotlin (Android)

Este ejemplo describe una posible arquitectura movil nativa Android en Kotlin para una cafeteria. Se organiza por capas de UI, dominio y datos para garantizar mantenimiento y escalabilidad.

## Infraestructura Tecnica

```text
cafeteria-android/
|-- app/
|   |-- src/main/
|   |   |-- AndroidManifest.xml
|   |   |-- java/com/cafeteria/
|   |   |   |-- ui/
|   |   |   |   |-- activities/
|   |   |   |   |-- fragments/
|   |   |   |   `-- viewmodels/
|   |   |   |-- data/
|   |   |   |   |-- local/
|   |   |   |   |-- remote/
|   |   |   |   `-- repository/
|   |   |   |-- domain/
|   |   |   |   |-- model/
|   |   |   |   `-- usecase/
|   |   |   `-- di/
|   |   `-- res/
|   |       |-- layout/
|   |       |-- values/
|   |       `-- drawable/
|   `-- build.gradle.kts
|-- build.gradle.kts
|-- settings.gradle.kts
`-- README.md
```

## Arquitectura Funcional

```text
cafeteria-android
|-- UI
|   |-- activities/
|   |-- fragments/
|   `-- adapters/
|-- ViewModels
|   `-- state y eventos
|-- Dominio
|   |-- usecases/
|   `-- modelos de negocio
|-- Datos
|   |-- repository/
|   |-- local/ (Room)
|   `-- remote/ (Retrofit)
|-- Inyeccion de dependencias
|   `-- Hilt / Dagger
|-- Recursos
    `-- layouts / drawables / strings
```

## Flujo Principal

```text
Usuario interactua UI
  -> ViewModel recibe evento
  -> UseCase ejecuta logica
  -> Repository consulta local/remote
  -> Datos retornan al ViewModel
  -> UI se actualiza
```

## Patron Interno De Un Modulo

```text
feature/
|-- ui/
|   |-- FeatureFragment.kt
|   `-- FeatureAdapter.kt
|-- viewmodel/
|   `-- FeatureViewModel.kt
|-- domain/
|   `-- FeatureUseCase.kt
|-- data/
|   |-- FeatureRepository.kt
|   |-- FeatureRemoteDataSource.kt
|   `-- FeatureLocalDataSource.kt
`-- models/
```

## Ejemplo Logico Del Dominio

```text
Pedido
  -> Carrito
  -> Cantidades
  -> Total
Menu
  -> Categorias
  -> Productos
Reserva
  -> Fecha
  -> Mesa
``