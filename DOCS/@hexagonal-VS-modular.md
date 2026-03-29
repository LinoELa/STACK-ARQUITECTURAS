# Hexagonal Vs Modular

Este documento compara `Hexagonal` y `Modular`, explica sus diferencias principales y orienta sobre cuando conviene usar una u otra.

---

## 1. Idea corta

Las dos sirven para ordenar un proyecto, pero no organizan el sistema con el mismo foco.

### Hexagonal

Pone el centro en:

- el dominio
- los casos de uso
- la separacion entre negocio y tecnologia

### Modular

Pone el centro en:

- bloques funcionales
- modulos del sistema
- claridad para crecer por areas o features

---

## 2. Diferencia principal

La diferencia mas importante es esta:

- `Hexagonal` prioriza la direccion de dependencias
- `Modular` prioriza la organizacion funcional del sistema

En otras palabras:

- Hexagonal pregunta: "como protejo el negocio de la tecnologia"
- Modular pregunta: "como divido el sistema en bloques claros"

---

## 3. Mi criterio

`Modular` suele ser mejor para la mayoria de proyectos reales, sobre todo si quieres:

- empezar rapido
- separar por funcionalidades
- mantener el codigo entendible
- escalar sin meter demasiada complejidad

`Hexagonal` suele ser mejor si:

- el dominio tiene bastante logica de negocio
- quieres desacoplar mucho la aplicacion de la base de datos, APIs o framework
- necesitas alta testabilidad
- el proyecto va a crecer bastante o cambiar de integraciones con el tiempo

La parte critica es esta:

- `Hexagonal` no compite con `Modular`
- muchas veces se combinan

Lo mas sano suele ser:

- arquitectura modular por features
- y dentro de los modulos importantes aplicar principios hexagonales

Ejemplo:

- modulo usuarios
- modulo pedidos
- modulo pagos

Y dentro de `pagos`:

- dominio
- casos de uso
- puertos
- adaptadores

Asi tienes orden sin complicarte todo el proyecto desde el dia 1.

### Mi recomendacion directa

- proyecto pequeno o mediano, equipo pequeno, necesidad de ir rapido: `Modular`
- proyecto con reglas de negocio fuertes, vida larga, varias integraciones, necesidad de tests serios: `Modular + Hexagonal dentro`
- hacer todo hexagonal desde el principio en un CRUD simple: normalmente es sobreingenieria

### Error tipico

Mucha gente mete `Hexagonal` solo porque suena mas serio o mas profesional.

El problema es que, si el proyecto todavia es simple, eso suele terminar en:

- capas vacias
- puertos innecesarios
- wiring excesivo
- mas complejidad sin beneficio real

---

## 4. Como suele verse cada una

### Hexagonal

```text
feature/
|-- domain/
|-- application/
`-- infrastructure/
```

### Modular

```text
module/
|-- module.module.ts
|-- action-a/
|   |-- action-a.controller.ts
|   |-- action-a.dto.ts
|   `-- action-a.service.ts
`-- action-b/
```

---

## 5. Que gana cada enfoque

### Hexagonal gana en

- aislamiento del dominio
- testeabilidad del negocio
- bajo acoplamiento con framework u ORM
- claridad en la dependencia entre capas

### Modular gana en

- lectura rapida por bloques
- crecimiento por areas funcionales
- reparto del trabajo por equipos
- estructura facil de entender en proyectos de UI o apps con muchas vistas

---

## 6. Cuando usar Hexagonal

Usa `Hexagonal` cuando:

- el negocio tiene peso real
- quieres proteger reglas de negocio de la tecnologia
- tienes casos de uso claros
- preves cambios de framework, adaptadores o persistencia
- quieres separar muy bien dominio, aplicacion e infraestructura

Suele encajar mejor en:

- backend con reglas de negocio importantes
- frontend o mobile con dominio fuerte y operaciones complejas
- sistemas que necesitan buena longevidad arquitectonica

---

## 7. Cuando usar Modular

Usa `Modular` cuando:

- quieres dividir el sistema en bloques funcionales claros
- la prioridad es ordenar features y areas de trabajo
- el equipo necesita una estructura muy legible
- no necesitas tanta pureza de capas como en Hexagonal
- el proyecto crece por vistas, pantallas o modulos operativos

Suele encajar mejor en:

- frontend con varias areas de producto
- mobile con muchas screens y flows
- backend tipo panel, API administrativa o servicio por modulos

---

## 8. Cuando no basta una sola

No siempre hay que elegir una sola de forma estricta.

Tambien se pueden combinar:

- Modular por fuera
- Hexagonal por dentro

Ejemplo:

```text
modules/
|-- auth/
|   |-- domain/
|   |-- application/
|   `-- infrastructure/
|-- workspace/
|   |-- domain/
|   |-- application/
|   `-- infrastructure/
```

Aqui el sistema es modular en la raiz, pero cada modulo sigue una estructura hexagonal interna.

---

## 9. Recomendacion practica

Si tienes dudas:

- usa `Modular` si tu dolor principal es el desorden por features
- usa `Hexagonal` si tu dolor principal es el acoplamiento entre negocio y tecnologia

Si el proyecto va a crecer mucho y tiene negocio fuerte:

- usa `Modular + Hexagonal`

---

## 10. Resumen final

En corto:

- `Hexagonal` protege el negocio
- `Modular` ordena el sistema por bloques
- `Hexagonal` piensa en capas y dependencias
- `Modular` piensa en modulos y areas funcionales
- no compiten siempre; muchas veces se complementan
