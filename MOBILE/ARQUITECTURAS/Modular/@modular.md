# Carpeta Modular

Este archivo explica que representa esta carpeta `Modular` dentro de `MOBILE`.

---

## 1. Que es esta carpeta

La carpeta `Modular` guarda referencias de arquitectura movil organizadas por bloques funcionales.

Aqui la idea principal es pensar primero en modulos de la app y despues en los archivos internos de cada screen o accion.

---

## 2. Que archivos tiene

Esta carpeta contiene:

- `@modular.md`
- `Arquitectura-Modular-MVP.md`
- `Arquitectura-Modular-Completo.md`

---

## 3. Como se lee esta arquitectura

La lectura correcta es esta:

- primero se mira el bloque principal
- luego el submodulo
- despues la accion o screen
- al final los archivos que resuelven esa accion

En mobile modular, la unidad de trabajo suele ser una `screen`, un `hook` y un `service`.

---

## 4. Regla principal

El wiring y composicion del bloque queda en `*.module.ts`.

La arquitectura crece por:

- bloques
- submodulos
- acciones

---

## 5. Diferencia entre MVP y Completo

### MVP

La version MVP usa menos bloques y menos submodulos.

### Completo

La version completa separa mejor session, workspace, history y settings.

---

## 6. Resumen rapido

La carpeta `Modular` de `MOBILE` sirve para mostrar como organizar una app movil por bloques funcionales, screens y acciones.
