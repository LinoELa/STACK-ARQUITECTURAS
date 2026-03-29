# Dependency Rule

La regla de dependencias de este template es:

```text
infrastructure -> application -> domain
```

## Permitido

- `application` puede usar contratos del `domain`
- `infrastructure` puede usar casos de uso de `application`
- `infrastructure` puede implementar puertos definidos en `domain`

## No permitido

- `domain` importando Prisma, Express, NestJS, Flask o Spring
- `domain` importando controllers, routes o middlewares
- `application` dependiendo directamente de una base de datos concreta
- un modulo leyendo directamente la persistencia interna de otro
- un adaptador tecnico definiendo reglas de negocio que deberian vivir en `domain`

## Flujo recomendado

```text
Route
  -> Controller
  -> Use Case
  -> Domain Port
  -> Repository / Adapter
  -> Response
```

## Objetivo

Esta regla existe para que:

- el negocio sea testeable
- el framework pueda cambiar sin romper el dominio
- los modulos crezcan con menos acoplamiento
- la tecnologia quede en los bordes y no en el centro
