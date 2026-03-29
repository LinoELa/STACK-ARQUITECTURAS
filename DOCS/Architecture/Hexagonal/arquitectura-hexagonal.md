# Arquitectura Hexagonal

Este template usa Arquitectura Hexagonal para separar la logica del negocio de los detalles tecnicos.

La idea principal es esta:

- `domain` contiene el negocio
- `application` orquesta casos de uso
- `infrastructure` implementa detalles tecnicos

La arquitectura debe cumplir estas reglas:

- el dominio no depende de framework, ORM ni librerias externas
- la aplicacion depende de puertos del dominio
- la infraestructura implementa esos puertos
- las dependencias siempre apuntan hacia el dominio

Una estructura habitual de cada modulo es:

```text
module/
|-- domain/
|-- application/
`-- infrastructure/
```

Si el proyecto crece, se puede combinar con:

- Vertical Slicing
- Screaming Architecture
- CQRS ligero
- eventos de dominio

Este documento es la vista general.
Las reglas concretas viven en los otros archivos de `DOCS/Architecture/Hexagonal/`.
