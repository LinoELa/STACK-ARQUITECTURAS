# Audit And Mapping

Este archivo sirve para revisar un proyecto existente y mapearlo hacia la arquitectura objetivo.

## 1. Inventario actual

Completar:

- modulos actuales:
- rutas actuales:
- servicios actuales:
- repositorios actuales:
- integraciones externas:
- middlewares globales:

## 2. Mapeo hacia slices

Por cada parte actual, indicar a que slice deberia pertenecer:

- `auth`:
- `workspace`:
- `compare`:
- otro:

## 3. Mapeo hacia capas

Clasificar cada pieza existente en una de estas capas:

- `domain`
- `application`
- `infrastructure`

## 4. Hallazgos comunes

Buscar si existen estos problemas:

- logica de negocio metida en controllers
- acceso a base de datos desde casos de uso
- validaciones HTTP mezcladas con reglas del dominio
- dependencias cruzadas entre modulos
- carpeta `shared` creciendo sin criterio
- hooks o pages con logica de negocio que deberia vivir en casos de uso

## 5. Salida esperada

Al terminar la auditoria deberia quedar claro:

- que se conserva
- que se mueve
- que se separa
- que se elimina
- que se renombra
