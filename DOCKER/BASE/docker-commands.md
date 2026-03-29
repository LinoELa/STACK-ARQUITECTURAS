# Comandos Docker

Este archivo deja una lista corta de comandos base para trabajar con Docker en cualquier proyecto.

---

## 1. Arrancar el entorno

```bash
docker compose up --build
```

Construye y levanta los servicios.

---

## 2. Arrancar en segundo plano

```bash
docker compose up -d
```

Levanta los servicios sin bloquear la terminal.

---

## 3. Parar y borrar contenedores

```bash
docker compose down
```

Apaga los servicios del proyecto.

---

## 4. Ver logs

```bash
docker compose logs -f
```

Para un servicio concreto:

```bash
docker compose logs -f app
docker compose logs -f db
```

---

## 5. Entrar al contenedor

```bash
docker compose exec app sh
```

Si el contenedor usa bash:

```bash
docker compose exec app bash
```

---

## 6. Reconstruir desde cero

```bash
docker compose down
docker compose up --build
```

---

## 7. Ver estado

```bash
docker compose ps
docker image ls
docker container ls
```

---

## 8. Borrar volumenes del proyecto

```bash
docker compose down -v
```

Usalo solo cuando realmente quieras reiniciar datos locales.

---

## 9. Comandos que mas vas a repetir

Los mas comunes suelen ser:

- `docker compose up --build`
- `docker compose up -d`
- `docker compose down`
- `docker compose logs -f`
- `docker compose exec app sh`

---

## 10. Resumen rapido

Si quieres un flujo minimo de trabajo, piensa en esta secuencia:

1. `docker compose up --build`
2. `docker compose logs -f`
3. `docker compose exec app sh`
4. `docker compose down`
