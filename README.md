# Laravel Docker Environment

Este proyecto contiene un entorno de desarrollo para **Laravel** utilizando **Docker**, **Nginx**, **PHP-FPM**, **PostgreSQL** y **Node.js**.

El objetivo es poder levantar un proyecto Laravel sin necesidad de instalar PHP, PostgreSQL o Node directamente en la máquina local.

---

# Requisitos

Antes de comenzar necesitas tener instalado:

* Docker
* Docker Compose
* Git

En Windows es recomendable instalar **Docker Desktop** con **WSL2** habilitado.

---

# Clonar el repositorio

```bash
git clone https://github.com/anthonycampoc/laravel-docker.git
cd laravel-docker
```

---

# Levantar los contenedores

Construir y levantar el entorno:

```bash
docker compose up -d --build
```

Esto iniciará los siguientes servicios:

* PHP (Laravel)
* Nginx
* PostgreSQL
* Node

---

# Instalar dependencias de Laravel

```bash
docker compose exec php composer install
```

---

# Crear archivo de entorno

```bash
docker compose exec php cp .env.example .env
```

---

# Generar la clave de la aplicación

```bash
docker compose exec php php artisan key:generate
```

---

# Configuración de Base de Datos

El proyecto está configurado para usar **PostgreSQL** dentro del contenedor Docker.

Configuración en `.env`:

```
DB_CONNECTION=pgsql
DB_HOST=postgres
DB_PORT=5432
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=secret
```

---

# Ejecutar migraciones

```bash
docker compose exec php php artisan migrate
```

---

# Instalar dependencias de frontend

```bash
docker compose run node npm install
```

---

# Compilar assets (Vite)

```bash
docker compose run node npm run dev
```

---

# Acceder a la aplicación

Una vez levantado el entorno, abre en tu navegador:

```
http://localhost:8080
```

---

# Comandos útiles

### Ver contenedores activos

```bash
docker compose ps
```

### Entrar al contenedor PHP

```bash
docker compose exec php bash
```

### Ejecutar comandos Artisan

```bash
docker compose exec php php artisan
```

Ejemplo:

```bash
docker compose exec php php artisan migrate
```

---

# Detener contenedores

```bash
docker compose down
```

---

# Servicios del entorno

| Servicio | Descripción                    |
| -------- | ------------------------------ |
| nginx    | Servidor web                   |
| php      | PHP-FPM ejecutando Laravel     |
| postgres | Base de datos PostgreSQL       |
| node     | Compilación de assets con Vite |

---

# Estructura del proyecto

```
laravel-docker
│
├── docker
│   ├── nginx
│   │   └── default.conf
│   └── php
│       └── Dockerfile
│
├── src
│   └── (proyecto Laravel)
│
└── docker-compose.yml
```

---

# Notas

* No es necesario usar `php artisan serve` porque el proyecto utiliza **Nginx**.
* Laravel se ejecuta a través de **PHP-FPM** dentro del contenedor.
* El servidor web expone el proyecto en el puerto **8080**.

---

# Autor

Repositorio creado para practicar y trabajar con **Laravel + Docker**.
