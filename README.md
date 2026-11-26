# GalaxyCell

Plataforma en desarrollo para la gestión de un catálogo de productos de telefonía y tecnología. El repositorio reúne una API REST, una aplicación Android y una interfaz web en construcción.

## Componentes

| Componente | Tecnologías | Estado visible en el repositorio |
| --- | --- | --- |
| `back-end/` | NestJS, TypeScript, TypeORM, PostgreSQL | API para productos, marcas, categorías e imágenes |
| `Android/` | Kotlin, Jetpack Compose, Coil | Aplicación Android |
| `Web/` | React, TypeScript, Vite, Tailwind CSS | Base web de Vite; aún conserva la pantalla de ejemplo |
| `docker-compose.yml` | Docker Compose, PostgreSQL 15 | Orquestación de API y base de datos |

## Funcionalidades de la API

- Crear, consultar, buscar, actualizar y eliminar lógicamente productos.
- Filtrar productos por nombre, categoría o marca, con paginación.
- Administrar marcas y categorías.
- Subir imágenes JPG, JPEG, PNG y WebP; quedan disponibles en `/uploads/`.
- Validar variables de entorno al iniciar y validar los datos de entrada.

Las rutas están organizadas bajo `/product`, `/brand`, `/category` y `/images`. Consulta los controladores en `back-end/src/` para ver los métodos y rutas exactos.

## Requisitos

- Node.js 20 o superior y pnpm (Corepack puede habilitarlo).
- Docker con Docker Compose, si se desea ejecutar PostgreSQL en contenedor.
- Android Studio y JDK 17 para compilar la aplicación Android.

## Inicio rápido con Docker

El servicio de API espera un archivo `back-end/.env`. Créalo antes de iniciar los contenedores:

```dotenv
NODE_ENV=development
PORT=3000
DB_HOST=db
DB_PORT=5432
DB_USERNAME=postgres
DB_PASSWORD=password
DB_DATABASE=nestdb
SALTROUNDS=10
TOKEN_SECRET=cambia-este-secreto
```

Luego, desde la raíz del repositorio:

```bash
docker compose up --build
```

La API queda en `http://localhost:3000`. PostgreSQL queda publicado en el puerto `5435` del host y conserva sus datos en el volumen `postgres_data`. Las imágenes se guardan en `back-end/uploads/`.

Para detener los servicios:

```bash
docker compose down
```

El comando anterior conserva el volumen de la base de datos. Para eliminar también los datos persistidos, usa `docker compose down -v`.

## Ejecutar la API localmente

Configura `back-end/.env` con las mismas variables. Para una base local, cambia `DB_HOST` a `localhost` y usa el puerto correspondiente (por ejemplo, `5435` si utilizas el servicio `db` de Docker Compose).

```bash
cd back-end
pnpm install
pnpm run start:dev
```

La API escucha en el puerto indicado por `PORT` (3000 por defecto en el bootstrap). Hay comandos disponibles para compilar y ejecutar en producción:

```bash
pnpm run build
pnpm run start:prod
```

## Ejecutar la aplicación web

```bash
cd Web
pnpm install
pnpm run dev
```

Vite mostrará en la terminal la URL local. También están disponibles `pnpm run build`, `pnpm run preview` y `pnpm run lint`. La interfaz actual es el esqueleto inicial de React + Vite.

## Compilar Android

Abre la carpeta `Android/` en Android Studio y sincroniza Gradle. El proyecto usa JDK 17, Android Gradle Plugin 8.13.2, `minSdk 29` y `compileSdk 36`. También puedes compilar desde la carpeta `Android/` en Windows:

```powershell
./gradlew.bat assembleDebug
```

## Rutas principales

| Recurso | Rutas |
| --- | --- |
| Productos | `GET/POST /product`, `GET/PATCH/DELETE /product/:id`, `GET /product/name/:name`, `GET /product/category/:category`, `GET /product/brand/:brand` |
| Marcas | `GET/POST /brand`, `GET/PATCH/DELETE /brand/:id` |
| Categorías | `GET /category`, `GET /category/:id`, `POST /category/create`, `PATCH /category/update/:id`, `DELETE /category/delete/:id` |
| Imágenes | `POST /images` (campo multipart `image`), `GET /images`; archivos servidos desde `/uploads/` |

Las operaciones `DELETE` de productos, marcas y categorías son eliminaciones lógicas.

## Estructura del repositorio

```text
.
├── Android/             # Aplicación Android nativa
├── back-end/            # API NestJS y Dockerfile
├── Web/                 # Interfaz React + Vite
├── docker-compose.yml   # API y PostgreSQL
└── README.md
```

## Notas de configuración

- TypeORM tiene `synchronize: true` en la configuración actual. Es práctico durante el desarrollo; evalúa migraciones antes de usar datos de producción.
- Cambia `TOKEN_SECRET` y las credenciales de PostgreSQL por valores seguros antes de desplegar. No publiques el archivo `.env`.
- El CORS de la API está habilitado para cualquier origen en la configuración actual; ajústalo según el entorno de despliegue.

## Licencia

No se ha definido una licencia para este proyecto.
