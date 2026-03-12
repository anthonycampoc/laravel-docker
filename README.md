# Laravel Docker Development Environment

Este repositorio proporciona un entorno de desarrollo para **Laravel** utilizando contenedores **Docker**.  
El objetivo es poder ejecutar un proyecto Laravel sin instalar PHP, PostgreSQL o Node en el sistema local.

El entorno incluye:

* PHP 8.3 (PHP-FPM)
* Nginx
* PostgreSQL
* Node.js
* Composer

---

# Requisitos

Antes de comenzar debes tener instalado:

* Docker
* Docker Compose

En Windows lo más común es usar **Docker Desktop**.

---

# Estructura del proyecto

```
laravel-docker
│
├── docker
│   ├── nginx
│   │   └── default.conf
│   │
│   └── php
│       └── Dockerfile
│
├── src
│   └── (Proyecto Laravel)
│
├── docker-compose.yml
├── d.bat
└── README.md
```

---

# Explicación de los contenedores

El archivo `docker-compose.yml` define los siguientes servicios:

## PHP

Contenedor que ejecuta **PHP-FPM** y donde se ejecuta Laravel.

Responsabilidades:

* Ejecutar Laravel
* Ejecutar comandos Artisan
* Ejecutar Composer

También tiene instaladas extensiones necesarias como:

* pdo
* pdo_pgsql
* zip
* mbstring
* exif
* pcntl
* bcmath

---

## Nginx

Servidor web que recibe las peticiones HTTP y las envía a PHP-FPM.

Puerto expuesto:

```
http://localhost:8080
```

Nginx sirve la carpeta:

```
/var/www/public
```

---

## PostgreSQL

Base de datos del proyecto.

Configuración por defecto:

```
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=secret
```

Puerto expuesto en la máquina local:

```
5433
```

---

## Node

Contenedor usado para compilar assets del frontend.

Se utiliza para ejecutar:

```
npm install
npm run dev
```

---

# Volúmenes

El proyecto utiliza volúmenes para persistir datos y compartir archivos entre contenedores.

### Código del proyecto

```
./src:/var/www
```

Esto permite editar el código desde el host mientras se ejecuta dentro del contenedor.

---

### Base de datos

```
pgdata:/var/lib/postgresql/data
```

Este volumen guarda los datos de PostgreSQL para evitar que se pierdan al reiniciar contenedores.

---

# Crear el proyecto Laravel

Si el directorio `src` está vacío, puedes crear un proyecto Laravel con:

```
docker compose run --rm php composer create-project laravel/laravel .
```

Esto instalará Laravel dentro de la carpeta `src`.

---

# Levantar los contenedores

Desde la raíz del proyecto ejecutar:

```
docker compose up -d --build
```

Esto iniciará todos los servicios.

Verificar que estén activos:

```
docker compose ps
```

---

# Configurar el archivo .env

Crear archivo de entorno:

```
docker compose exec php cp .env.example .env
```

---

# Generar clave de aplicación

```
docker compose exec php php artisan key:generate
```

Configurar la conexión a la base de datos:

```
DB_CONNECTION=pgsql
DB_HOST=postgres
DB_PORT=5432
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=secret
```

---

# Instalar dependencias

Instalar dependencias PHP con Composer:

```
docker compose exec php composer install
```

---

# Ejecutar migraciones

```
docker compose exec php php artisan migrate
```

---

# Acceder a la aplicación

Abrir en el navegador:

```
http://localhost:8080
```

---

# Uso de Node

Para instalar dependencias de Node:

```
docker compose exec node npm install
```

Compilar assets en desarrollo:

```
docker compose exec node npm run dev
```

Compilar assets para producción:

```
docker compose exec node npm run build
```

---

# Script d.bat

El proyecto incluye el archivo `d.bat`, que sirve como un atajo para ejecutar comandos dentro del contenedor PHP.

Contenido del archivo:

```
@echo off
docker compose exec php %*
```

---

# Cómo usar d.bat

En lugar de escribir comandos largos como:

```
docker compose exec php php artisan migrate
```

puedes ejecutar:

```
.\d php artisan migrate
```

Otros ejemplos:

Instalar dependencias:

```
.\d composer install
```

Crear un modelo:

```
.\d php artisan make:model Post -m
```

Abrir terminal dentro del contenedor:

```
.\d bash
```

---

# Comandos útiles

Ver logs de contenedores:

```
docker compose logs -f
```

Entrar al contenedor PHP:

```
docker compose exec php bash
```

Ver contenedores activos:

```
docker compose ps
```

---

# Detener contenedores

```
docker compose down
```

---

# Reconstruir contenedores

Si realizas cambios en el Dockerfile:

```
docker compose up -d --build
```

---

# Notas

Este entorno está diseñado para aprendizaje y desarrollo local con Docker y Laravel.

Permite trabajar sin instalar PHP, PostgreSQL o Node directamente en el sistema operativo.
