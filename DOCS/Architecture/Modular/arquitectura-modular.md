# Arquitectura Modular

Este template usa Arquitectura Modular para dividir el sistema en bloques funcionales claros.

La idea principal es esta:

- el sistema se organiza por modulos
- cada modulo encapsula una responsabilidad
- cada modulo puede crecer con submodulos y acciones internas

Una estructura habitual de cada bloque es:

```text
module/
|-- module.module.ts
|-- action-a/
`-- action-b/
```

Si el proyecto crece, se puede combinar con:

- Vertical Slicing
- Hexagonal interna por modulo
- CQRS ligero
- diseño por flujos o areas funcionales

Este documento es la vista general.
Las reglas concretas viven en los otros archivos de `DOCS/Architecture/Modular/`.
