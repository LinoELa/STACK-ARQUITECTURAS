# Audit And Mapping

Este archivo sirve para revisar un proyecto existente y mapearlo hacia una arquitectura modular.

## 1. Inventario actual

Completar:

- modulos actuales:
- vistas o pantallas actuales:
- servicios actuales:
- hooks o controladores actuales:
- integraciones externas:
- piezas compartidas:

## 2. Mapeo hacia bloques

Por cada parte actual, indicar a que bloque o modulo deberia pertenecer:

- `core`:
- `session`:
- `workspace`:
- `history`:
- `settings`:
- otro:

## 3. Mapeo hacia submodulos y acciones

Clasificar cada pieza existente en una de estas unidades:

- modulo
- submodulo
- accion o vista
- shared

## 4. Hallazgos comunes

Buscar si existen estos problemas:

- demasiadas carpetas globales por tecnologia
- servicios de varios modulos mezclados
- screens, pages o endpoints sin bloque dueño
- logica de varios flujos en un solo archivo
- `shared` creciendo sin criterio

## 5. Salida esperada

Al terminar la auditoria deberia quedar claro:

- que bloques existen
- que modulo es dueño de cada flujo
- que piezas deben quedarse juntas
- que piezas deben salir a `shared`
- que se renombra o se separa
