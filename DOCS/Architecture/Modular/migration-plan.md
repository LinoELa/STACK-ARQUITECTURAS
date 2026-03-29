# Migration Plan

Este documento sirve como plantilla de migracion hacia una arquitectura modular.

## Fase 1. Entender el punto de partida

- listar modulos o areas actuales
- listar carpetas globales por tecnologia
- identificar flujos principales
- detectar archivos demasiado grandes o mezclados

## Fase 2. Definir bloques

- decidir bloques funcionales principales
- acordar nombres claros por negocio o flujo
- definir que parte del sistema es dueña de cada responsabilidad

## Fase 3. Separar modulos

- mover vistas, screens, controllers u operaciones al modulo correcto
- crear submodulos cuando el bloque lo necesite
- agrupar acciones relacionadas bajo el mismo bloque

## Fase 4. Ordenar shared

- dejar en `shared` solo piezas transversales reales
- sacar de `shared` logica que deba volver a un modulo
- revisar utilidades, componentes y clientes comunes

## Fase 5. Revisar dependencias

- comprobar que los modulos no se crucen de forma caotica
- comprobar que cada bloque tenga dueño claro
- comprobar que la lectura del repo siga siendo simple

## Fase 6. Cerrar migracion

- actualizar documentacion
- revisar tests
- ajustar nombres de bloques y submodulos
- dejar clara la nueva estructura final
