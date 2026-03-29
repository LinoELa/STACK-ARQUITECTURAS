# Dockerfile

Este archivo explica que papel cumple el `Dockerfile` y cual es su forma base.

---

## 1. Para que sirve

El `Dockerfile` define como se construye la imagen del proyecto.

Normalmente responde estas preguntas:

- que imagen base se usa
- en que carpeta trabaja el contenedor
- que dependencias se instalan
- que archivos se copian
- que puerto expone
- que comando arranca la aplicacion

---

## 2. Forma minima

```dockerfile
FROM <runtime-image>

WORKDIR /app

COPY <dependency-files> ./
RUN <install-command>

COPY . .

EXPOSE <port>

CMD ["<start-command>"]
```

---

## 3. Orden recomendado

La secuencia mas comun es esta:

1. elegir la imagen base
2. definir `WORKDIR`
3. copiar archivos de dependencias
4. instalar dependencias
5. copiar el resto del proyecto
6. exponer el puerto si aplica
7. definir `CMD`

Ese orden ayuda a aprovechar mejor la cache de Docker.

---

## 4. Regla de cache

La idea base es:

- copiar primero `package.json`, `requirements.txt`, `pom.xml` o equivalente
- instalar dependencias
- copiar despues el resto del codigo

Asi no reinstalas todo cada vez que cambias un archivo de `src/`.

---

## 5. Dev y prod

No siempre hace falta separar.

Para un proyecto simple, un solo `Dockerfile` suele bastar.

Cuando el proyecto crece, puedes pasar a:

- `Dockerfile`
- `Dockerfile.dev`
- `Dockerfile.prod`

O tambien a un solo `Dockerfile` con etapas.

---

## 6. Que no deberia hacer

Evita estas practicas:

- copiar secretos dentro de la imagen
- usar una imagen base demasiado pesada sin necesidad
- instalar todo antes de copiar archivos clave de dependencias
- meter comandos enormes y dificiles de leer

---

## 7. Resumen rapido

El `Dockerfile` es la receta de construccion de la imagen.

Su forma minima suele ser:

- `FROM`
- `WORKDIR`
- `COPY`
- `RUN`
- `EXPOSE`
- `CMD`
