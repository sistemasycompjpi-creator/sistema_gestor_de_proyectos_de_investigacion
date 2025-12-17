# Guía de Contribución y Configuración

Esta guía detalla el proceso de instalación y configuración del entorno de desarrollo para el Sistema de Gestión de Proyectos de Investigación. El proyecto utiliza Laravel como framework Full Stack, lo que implica una configuración tanto de Backend (PHP) como de Frontend (Node.js).

---

## Fase 1: Prerrequisitos

Antes de comenzar, asegúrate de tener instalado lo siguiente en tu entorno de desarrollo:

1.  **XAMPP:** Para el servidor web y PHP.
    *   *Nota:* Recuerda activar las extensiones `zip`, `pdo_pgsql`, y `pgsql` en tu archivo `php.ini`.
2.  **Composer:** Gestor de dependencias de PHP (equivalente a `npm` pero para el backend).
3.  **PostgreSQL:** Motor de base de datos.
4.  **Node.js & NPM:** Para la compilación de estilos y scripts (Vite).
5.  **Git:** Para el control de versiones.

---

## Fase 2: Instalación y Despliegue

Sigue estos pasos estrictamente para levantar el proyecto en un entorno local.

### 1. Clonar el repositorio

Navega a tu carpeta de servidor (ej. `htdocs`) y clona el proyecto:

```bash
cd C:\xampp\htdocs
git clone <URL_DEL_REPOSITORIO>
cd sistema-gestion-investigacion
```

### 2. Instalar dependencias

El proyecto requiere instalar dependencias tanto para el backend como para el frontend.

*   **Backend (PHP/Laravel):**
    ```bash
    composer install
    ```

*   **Frontend (JS/CSS):**
    ```bash
    npm install
    ```

### 3. Configuración de Entorno (.env)

El archivo `.env` contiene credenciales sensibles y no se incluye en el repositorio. Debes crear una copia del archivo de ejemplo:

```bash
cp .env.example .env
# O en CMD de Windows: copy .env.example .env
```

### 4. Configurar Base de Datos

Edita el archivo `.env` recién creado con tus credenciales de PostgreSQL locales:

```env
DB_CONNECTION=pgsql
DB_HOST=127.0.0.1
DB_PORT=5432
DB_DATABASE=gestion_investigacion
DB_USERNAME=postgres
DB_PASSWORD=TU_CONTRASEÑA
```

### 5. Generar Key de Aplicación

Genera la llave de encriptación única para tu instancia local:

```bash
php artisan key:generate
```

### 6. Base de Datos y Migraciones

1.  Crea una base de datos vacía llamada `gestion_investigacion` usando pgAdmin o tu cliente SQL preferido.
2.  Ejecuta las migraciones para crear la estructura de tablas:

```bash
php artisan migrate
```

### 7. Compilar y Correr

Para iniciar el entorno de desarrollo:

1.  Compilar assets (Frontend):
    ```bash
    npm run build
    ```
    *Para desarrollo en vivo, usa `npm run dev` en una terminal aparte.*

2.  Iniciar servidor (Backend):
    ```bash
    php artisan serve
    ```

---

## Arquitectura: Laravel + Node.js

Es importante entender cómo conviven PHP y Node.js en este proyecto.

### Roles

| Herramienta | Rol | Archivo de Configuración | Descripción |
| :--- | :--- | :--- | :--- |
| **Composer (PHP)** | Backend | `composer.json` | Instala Laravel, drivers de BD, utilidades de servidor. Se ejecuta en Apache/PHP. |
| **NPM (Node.js)** | Frontend | `package.json` | Instala Tailwind, librerías JS, Vite. Se usa solo durante desarrollo/compilación. |

### Flujo de Trabajo (Vite)

Apache (XAMPP) sirve la aplicación, pero no procesa el JavaScript/CSS moderno directamente.

1.  **Node.js (Vite)** compila los recursos en `resources/` (SCSS, JS moderno).
2.  Genera archivos estáticos optimizados en `public/build/`.
3.  **Laravel** sirve estos archivos compilados al navegador.

### Comandos Frontend

*   **`npm run dev`**: Levanta un servidor de desarrollo local. Permite "Hot Module Replacement" (cambios en tiempo real sin recargar).
*   **`npm run build`**: Compila los assets para producción. Minifica CSS/JS y genera los archivos finales en `public/`. **Este comando es necesario antes de desplegar o si no vas a usar el servidor de desarrollo.**

---

### Notas Importantes

*   **Datos:** Al clonar y migrar, la base de datos tendrá la estructura correcta pero estará **vacía**. Git solo versiona el código, no los datos de tu base local.
