# .dockerignore

Este archivo explica para que sirve `.dockerignore` y que suele incluir.

---

## 1. Para que sirve

`.dockerignore` evita que Docker envie archivos innecesarios al contexto de build.

Eso ayuda a:

- acelerar el build
- reducir peso
- evitar copiar basura del proyecto
- no meter archivos sensibles por error

---

## 2. Base comun

```text
.git
.gitignore
.vscode
.idea

.env
.env.*

node_modules
dist
build
coverage

__pycache__
*.pyc
.venv

target
.gradle

.dart_tool
```

---

## 3. Regla importante

No copies esta lista a ciegas.

La idea es:

- dejar solo lo que tenga sentido para el stack real
- ignorar artefactos generados
- ignorar secretos locales
- no ignorar archivos que el build realmente necesita

---

## 4. Casos comunes

En Node o JavaScript suele aparecer:

- `node_modules`
- `dist`
- `coverage`

En Python suele aparecer:

- `__pycache__`
- `*.pyc`
- `.venv`

En Java o Kotlin suele aparecer:

- `target`
- `build`
- `.gradle`

En Flutter suele aparecer:

- `.dart_tool`
- `build`

---

## 5. Resumen rapido

`.dockerignore` existe para que la imagen se construya con menos ruido.

La regla simple es:

- ignorar artefactos
- ignorar secretos locales
- ignorar carpetas pesadas que no deben copiarse
