# Migration Plan

Este documento sirve como plantilla de migracion hacia la arquitectura objetivo.

## Fase 1. Entender el punto de partida

- listar modulos actuales
- listar dependencias tecnicas
- identificar flujos principales
- detectar acoplamientos fuertes

## Fase 2. Definir slices

- decidir modulos de negocio principales
- acordar nombres de cada slice
- mover responsabilidades al modulo correcto

## Fase 3. Separar capas

- extraer entidades y contratos al `domain`
- mover casos de uso a `application`
- dejar rutas, controllers y repositorios concretos en `infrastructure`

## Fase 4. Ordenar infraestructura

- aislar persistencia
- aislar seguridad
- aislar validaciones HTTP
- centralizar wiring por modulo
- separar tambien router, storage o platform adapters si aplica

## Fase 5. Revisar dependencias

- comprobar que `domain` no conoce tecnologia
- comprobar que `application` no depende de detalles concretos
- comprobar que los modulos no rompen la regla de dependencias

## Fase 6. Cerrar migracion

- actualizar documentacion
- ajustar seeds y migraciones
- revisar tests
- dejar clara la nueva estructura final
- verificar que la UI o API no vuelva a absorber reglas del dominio
