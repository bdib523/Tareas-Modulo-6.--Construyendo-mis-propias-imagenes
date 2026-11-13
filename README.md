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
