# Carpeta Architecture

Este archivo explica la organizacion interna de `DOCS/Architecture`.

---

## 1. Que es esta carpeta

`Architecture/` guarda documentacion arquitectonica reusable del repositorio.

Ahora esta separada por estilos:

- `Hexagonal/`
- `Modular/`

---

## 2. Como se lee

La lectura recomendada es esta:

1. comparar enfoques en `DOCS/@hexagonal-VS-modular.md`
2. entrar en `Hexagonal/` o `Modular/`
3. leer primero `@hexagonal.md` o `@modular.md`
4. despues revisar overview, rules, dependency rule, audit y migration plan

---

## 3. Que contiene cada carpeta

Cada carpeta de arquitectura intenta repetir la misma idea:

- documento general de la arquitectura
- reglas practicas
- dependency rule
- plantilla de auditoria y mapeo
- plantilla de migracion

---

## 4. Resumen rapido

`Architecture/` ya no guarda todo mezclado.

Ahora separa la documentacion en:

- una rama para `Hexagonal`
- una rama para `Modular`

asi es mas facil comparar, aprender y reutilizar cada enfoque.
