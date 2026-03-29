# Hexagonal Rules

Estas son las reglas practicas del template.

## 1. Reglas de capas

- `domain` no importa nada de `application`
- `domain` no importa nada de `infrastructure`
- `application` no importa adaptadores tecnicos concretos
- `infrastructure` implementa puertos definidos hacia dentro

## 2. Reglas por modulo

- cada modulo representa una capacidad del negocio
- cada modulo debe tener `domain`, `application` e `infrastructure`
- el wiring del modulo debe quedar aislado en un `container`
- la validacion HTTP no debe mezclarse con las reglas del dominio

## 3. Reglas de shared

- `shared/` solo se usa para piezas realmente transversales
- lo que pertenezca a un modulo debe quedarse en ese modulo
- no usar `shared/` como cajon desastre

## 4. Reglas de persistencia

- el dominio define contratos
- los repositorios concretos viven en infraestructura
- la base de datos es un detalle tecnico
- migraciones y seeds viven fuera del dominio

## 5. Reglas de lectura

La estructura debe dejar claro:

- que hace el sistema
- que modulo toca cada flujo
- donde termina el negocio y donde empieza la tecnologia
