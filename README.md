# Task Manager Docker

Una aplicación completa para la gestión de tareas, construida con una arquitectura de microservicios y completamente contenida en Docker.

## 🏗 Arquitectura

El proyecto utiliza **Docker Compose** para orquestar los siguientes servicios:
- **API (Backend):** Desarrollada en Python utilizando **FastAPI** y **Jinja2Templates** (renderiza vistas HTML y expone endpoints de una API REST).
- **Base de datos:** Instancia de **PostgreSQL** (`postgres:16-alpine`) para almacenar el estado y la información de la aplicación de tareas.
- **Servidor Web (Nginx):** Funciona como un proxy inverso escuchando en el puerto local `8080`, enrutando el tráfico desde el cliente hacia la API en el backend.

## 🚀 Requisitos

- [Docker](https://www.docker.com/) 
- [Docker Compose](https://docs.docker.com/compose/)

## ⚙️ Instalación y Uso

1. **Clonar este repositorio** (si aún no lo tienes):
   ```bash
   git clone <url-del-repositorio>
   cd task-manager-docker
   ```

2. **Levantar los servicios:**
   Ejecuta el siguiente comando en la raíz del proyecto para construir y levantar los contenedores en modo "detached" (segundo plano):
   ```bash
   docker compose up -d --build
   ```

3. **Acceder a la aplicación:**
   Una vez que los contenedores estén funcionando y la base de datos haya inicializado (el servicio de la base de datos incluye un `healthcheck`), abre tu navegador web favorito y visita:
   ```
   http://localhost:8080
   ```

4. **Detener la aplicación:**
   Para detener y eliminar los contenedores, redes y mantener los volúmenes, ejecuta:
   ```bash
   docker compose down
   ```
   *Nota: La información de las tareas se persistirá gracias al volumen `db_data` asociado a PostgreSQL.*

## 🛣 Endpoints de la API REST

Además de la interfaz web en la raíz (`/`), la API está disponible en `/api/tasks` para operaciones CRUD y devuelve sus respuestas en formato JSON:

- `GET /api/tasks` - Lista todas las tareas.
- `POST /api/tasks` - Crea una nueva tarea (Se requiere objeto JSON con `title`, opcionalmente `description` y `status`).
- `PUT /api/tasks/{id}` - Actualiza una tarea existente.
- `DELETE /api/tasks/{id}` - Elimina una tarea por su id.

## 🗂 Estructura de carpetas

```text
task-manager-docker/
├── api/                  # Código fuente de FastAPI (Backend y HTML)
│   ├── app.py            # Lógica y enrutamiento del backend
│   ├── Dockerfile        # Imagen Docker de la API
│   ├── requirements.txt  # Dependencias Python
│   └── templates/        # Vistas de Jinja2 (.html)
├── nginx/                # Configuración de Servidor Web (Proxy Inverso)
│   ├── default.conf      # Config de enrutamiento Nginx
│   └── Dockerfile        # Imagen Docker de Nginx
├── docker-compose.yml    # Orquestación de contenedores y volúmenes
└── README.md             # Esta documentación
```