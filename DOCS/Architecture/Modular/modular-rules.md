# Modular Rules

Estas son las reglas practicas del template modular.

## 1. Reglas de bloques

- cada modulo representa una capacidad funcional del sistema
- cada modulo debe tener una responsabilidad clara
- el nombre del modulo debe gritar negocio o flujo
- no meter modulos genericos sin frontera clara

## 2. Reglas internas

- cada modulo puede tener submodulos
- cada submodulo puede tener acciones o vistas concretas
- el wiring debe quedar en `*.module`
- no mezclar demasiadas acciones distintas en un solo archivo

## 3. Reglas de shared

- `shared/` solo debe guardar piezas realmente transversales
- no mover a `shared` codigo que todavia pertenece a un modulo
- evitar que `shared` se convierta en un cajon desastre

## 4. Reglas de lectura

La estructura debe dejar claro:

- cuales son los bloques del sistema
- que hace cada modulo
- donde vive cada accion o vista
- que piezas son transversales y cuales no

## 5. Reglas de crecimiento

- primero crece el modulo
- despues el submodulo
- despues la accion
- no crear nuevas carpetas si todavia no aportan claridad
