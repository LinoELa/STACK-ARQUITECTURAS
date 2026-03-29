# Carpeta Hexagonal

Este archivo explica que representa esta carpeta `Hexagonal` dentro de `MOBILE`.

---

## 1. Que es esta carpeta

La carpeta `Hexagonal` guarda referencias de arquitectura movil basadas en:

- Hexagonal
- Vertical Slicing
- Screaming Architecture

La idea principal es organizar la app movil por slices del negocio y no primero por screens o clientes HTTP globales.

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

En mobile, la UI vive normalmente dentro de `infrastructure`, junto con storage seguro, navegacion, plataforma y clientes remotos.

---

## 4. Regla principal

La idea base es esta:

- `domain` no conoce React Native, Flutter ni SwiftUI
- `application` usa contratos del dominio
- `infrastructure` implementa HTTP, secure storage, navigation y UI

La direccion de dependencias es:

```text
infrastructure -> application -> domain
```

---

## 5. Diferencia entre MVP y Completo

### MVP

La version MVP usa menos slices y menos niveles internos.

### Completo

La version completa separa mejor screens, hooks, components, storage y estado.

---

## 6. Resumen rapido

La carpeta `Hexagonal` de `MOBILE` sirve para mostrar como organizar una app movil con negocio en el centro y tecnologia en los bordes.
