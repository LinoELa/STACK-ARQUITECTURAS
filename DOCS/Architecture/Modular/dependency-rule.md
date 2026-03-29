# Dependency Rule

La regla de dependencias en una arquitectura modular es mas simple que en Hexagonal, pero sigue necesitando orden.

La idea base es esta:

```text
shared -> usado solo para piezas transversales
module -> dueño de su logica funcional
```

## Permitido

- un modulo usando sus propios servicios, hooks, controllers o actions
- un submodulo usando piezas internas del mismo modulo
- usar `shared` para utilidades realmente comunes

## No permitido

- un modulo leyendo archivos internos privados de otro modulo sin frontera clara
- mover logica de negocio a `shared` solo por reutilizacion rapida
- mezclar acciones de varios modulos dentro de la misma carpeta
- crear dependencias cruzadas dificiles de seguir entre bloques

## Flujo recomendado

```text
Module
  -> Submodule
  -> Action
  -> Internal service
  -> Shared support only if needed
```

## Objetivo

Esta regla existe para que:

- cada bloque sea facil de entender
- el crecimiento del sistema siga siendo legible
- el trabajo en equipo se pueda repartir mejor
