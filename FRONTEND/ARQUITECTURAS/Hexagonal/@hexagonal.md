# Carpeta Hexagonal

Este archivo explica que representa esta carpeta `Hexagonal` dentro de `FRONTEND`.

---

## 1. Que es esta carpeta

La carpeta `Hexagonal` guarda referencias de arquitectura frontend basadas en:

- Hexagonal
- Vertical Slicing
- Screaming Architecture

La idea principal es organizar la aplicacion web por slices del negocio y no primero por tecnologia.

---

## 2. Que archivos tiene

Esta carpeta contiene:

- `@hexagonal.md`
- `Arquitectura-Hexagonal-MVP.md`
- `Arquitectura-Hexagonal-Completo.md`

---

## 3. Como se lee esta arquitectura

La lectura correcta es esta:

- primero se mira el slice funcional
- despues su `domain`
- despues su `application`
- al final su `infrastructure`

En frontend, la UI vive normalmente dentro de `infrastructure`, junto con adaptadores HTTP, router, estado o storage.

---

## 4. Regla principal

La idea base es esta:

- `domain` no conoce React ni Next.js
- `application` usa contratos del dominio
- `infrastructure` implementa HTTP, browser storage, router y UI

La direccion de dependencias es:

```text
infrastructure -> application -> domain
```

---

## 5. Diferencia entre MVP y Completo

### MVP

La version MVP usa menos slices y menos niveles internos.

### Completo

La version completa separa mejor pages, hooks, components, storage y estado.

---

## 6. Resumen rapido

La carpeta `Hexagonal` de `FRONTEND` sirve para mostrar como organizar una app web con negocio en el centro y tecnologia en los bordes.
