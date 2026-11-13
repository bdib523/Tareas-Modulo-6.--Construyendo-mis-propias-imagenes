# 🐳 Construyendo mis propias imágenes Docker

**Autor:** Badr Diboune El Wasmi

Este proyecto documenta el proceso de creación de imágenes Docker personalizadas, tanto a partir de un contenedor modificado manualmente (`docker commit`) como mediante un `Dockerfile` con PHP 7.4 + Apache.

---

## 📋 Índice

- [Parte 1: Arrancar el contenedor y hacer las instalaciones](#parte-1-arrancar-el-contenedor-y-hacer-las-instalaciones)
- [Parte 2: Crear la imagen y subirla a Docker Hub](#parte-2-crear-la-imagen-y-subirla-a-docker-hub)
- [Dockerfile con PHP 7.4 + Apache](#dockerfile-con-php-74--apache)
- [Resultado final](#-resultado-final)

---

## Parte 1: Arrancar el contenedor y hacer las instalaciones

### Paso 1: Arrancar el contenedor con Ubuntu 20.04

```bash
docker run -it --name mi_ubuntu ubuntu:20.04 /bin/bash
```

- `-it` te da una terminal interactiva dentro del contenedor.
- `--name badrubuntu` le pone un nombre para localizarlo después.

### Paso 2: Actualizar la lista de paquetes

```bash
apt update
```

### Paso 3: Instalar nano

```bash
apt install nano -y
```

### Paso 4: Instalar vim

```bash
apt install vim -y
```

### Paso 5: Instalar las herramientas de red

```bash
apt install inetutils-tools -y
```

### Paso 6: Instalar las herramientas DNS

```bash
apt install dnsutils -y
```

Esto te da `nslookup` y `dig`.

### Paso 7: Crear el usuario `badr`

```bash
adduser badr
```

### Paso 8: Comprobar que todo está bien (opcional pero recomendable)

```bash
id badr
```
uid=1000(badr) gid=1000(badr) groups=1000(badr)


### Paso 9: Salir del contenedor

```bash
exit
```

---

## Parte 2: Crear la imagen y subirla a Docker Hub

### Paso 10: Comprobar que tu contenedor existe

```bash
docker ps -a
```

Verás `badrubuntu` en la lista con estado `Exited`.

### Paso 11: Crear la imagen con `docker commit`

```bash
docker commit badrubuntu bdib523/a62
```

Esto guarda el estado del contenedor como una imagen llamada `bdib523/a62`.

### Paso 12: Comprobar que la imagen se creó

```bash
docker images
```

Debe aparecer `bdib523/a62` en la lista.

### Paso 13: Iniciar sesión en Docker Hub

```bash
docker login
```

Deberías ver `Login Succeeded`.

### Paso 14: Subir la imagen con `docker push`

```bash
docker push bdib523/a62
```

Comprobamos en [hub.docker.com](https://hub.docker.com) que aparece el repositorio `bdib523/a62` en nuestra cuenta.

---

## Dockerfile con PHP 7.4 + Apache

### Parte 1: Preparar la carpeta de trabajo

#### Paso 1: Crear una carpeta para el ejercicio

```bash
mkdir a63
cd a63
```

#### Paso 2: Crear el fichero `index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PRUEBA</title>
</head>
<body>
    <h1>HOLA SOY BADR USANDO DOCKER FILE</h1>
    <h2>en 2026</h2>
</body>
</html>
```

#### Paso 3: Crear el fichero `info.php`

```php
<?php phpinfo(); ?>
```

### Parte 2: Crear el Dockerfile

#### Paso 4: Crear el fichero `Dockerfile`

> ⚠️ El nombre debe ser exactamente `Dockerfile`, con la D mayúscula y sin extensión.

```dockerfile
FROM php:7.4-apache

RUN sed -i 's|deb.debian.org/debian-security|archive.debian.org/debian-security|g; \
s|deb.debian.org/debian|archive.debian.org/debian|g' /etc/apt/sources.list && \
    apt update && apt install -y nano && apt install -y git

COPY index.html /var/www/html/index.html
COPY info.php /var/www/html/info.php
```

**Qué hace cada línea:**

- `FROM php:7.4-apache`: parte de la imagen base que pide el enunciado.
- `RUN apt update && apt install -y nano && apt install -y git`: instala `nano` y `git`. El `apt update` previo es necesario para que encuentre los paquetes.
- `COPY ...`: copia los dos ficheros al directorio raíz de Apache (`/var/www/html`).

#### Paso 5: Comprobar que tienes los 3 ficheros

```bash
ls
```

Debe aparecer: `Dockerfile`, `index.html`, `info.php`

### Parte 3: Construir la imagen

#### Paso 6: Ejecutar `docker build`

```bash
docker build -t bdib523/a63 .
```

- `-t` pone el nombre a la imagen.
- El **punto final** indica que el `Dockerfile` está en la carpeta actual. No te lo olvides.

#### Paso 7: Comprobar que se creó

```bash
docker images
```

Debe aparecer `bdib523/a63`.

### Parte 4: Probarlo

#### Paso 8: Arrancar un contenedor con la imagen

```bash
docker run -d -p 8080:80 --name web_a63 bdib523/a63
```

#### Paso 9: Probar en el navegador

- [http://localhost:8080/index.html](http://localhost:8080/index.html)
- [http://localhost:8080/info.php](http://localhost:8080/info.php)

---

## ✅ Resultado final

Al visitar `http://localhost:8080/index.html` se muestra:

> **HOLA SOY BADR USANDO DOCKER FILE**
> en 2026

Al visitar `http://localhost:8080/info.php` se muestra la página estándar de `phpinfo()` con la información de PHP 7.4.33 y Apache 2.0 Handler.

---

## 🛠️ Tecnologías usadas

- Docker Desktop
- Ubuntu 20.04
- PHP 7.4 + Apache
- Docker Hub
