# Pet Manager

## Descripción General

Pet Manager es una solución integral basada en una arquitectura de microservicios diseñada para la gestión administrativa de proveedores, productos y notificaciones automatizadas. El sistema está construido utilizando Java y el ecosistema Spring Boot, implementando patrones de diseño modernos para garantizar escalabilidad, seguridad y mantenimiento.

El proyecto se divide en componentes independientes que se comunican entre sí, orquestados a través de un API Gateway centralizado.

## Arquitectura del Sistema

El repositorio está estructurado como un monorepositorio que contiene los siguientes servicios:

### 1. API Gateway (api-gateway)
Punto de entrada único para todas las solicitudes del cliente. Se encarga del enrutamiento de tráfico hacia los microservicios correspondientes y gestiona la configuración de CORS.

### 2. Auth Service (auth-service)
Servicio responsable de la seguridad y gestión de identidad.
* **Funcionalidades:** Registro de usuarios, inicio de sesión, recuperación de contraseñas y gestión de roles.
* **Seguridad:** Implementación de Spring Security y generación de tokens JWT (JSON Web Tokens).
* **API:** Expone endpoints tanto REST como GraphQL.

### 3. Supplier Service (supplier-service)
Núcleo de la lógica de negocio para la gestión de la cadena de suministro.
* **Entidades:** Gestión de Proveedores, Productos y Condiciones de Pago.
* **Interfaz:** Utiliza principalmente GraphQL para consultas flexibles y manipulación de datos.
* **Validaciones:** Incluye lógica para evitar duplicidad de recursos y validaciones de negocio.

### 4. Notification Service (notification-service)
Servicio dedicado a la comunicación asíncrona y tareas programadas.
* **Funcionalidades:** Envío de correos electrónicos y alertas de vencimiento de pagos.
* **Scheduling:** Tareas programadas (Schedulers) para verificar estados de pago automáticamente.
* **Integración:** Cliente Feign para comunicación inter-servicio con el Supplier Service.

## Tecnologías Utilizadas

* **Lenguaje:** Java (OpenJDK 17+)
* **Framework:** Spring Boot
* **Seguridad:** Spring Security, JWT
* **API:** GraphQL (Spring for GraphQL) y REST
* **Documentación:** Swagger / OpenAPI
* **Base de Datos:** SQL (Scripts de inicialización incluidos)
* **Contenedores:** Docker
* **Construcción:** Maven

## Requisitos Previos

Para ejecutar este proyecto localmente, asegúrese de tener instalado:

1. Java Development Kit (JDK) 17 o superior.
2. Maven (o utilizar el wrapper `mvnw` incluido en cada servicio).
3. Docker y Docker Compose (opcional, para despliegue en contenedores).
4. Un servidor de base de datos MySQL o compatible.

## Configuración y Base de Datos

El proyecto incluye una carpeta `scripts/` con archivos esenciales para la configuración de la base de datos:

* `init-db.sql`: Script inicial para la creación de esquemas y tablas necesarias.
* `aiven-migration.sql`: Scripts de migración para entornos en la nube (Aiven).

Antes de iniciar los servicios, asegúrese de configurar las variables de entorno o actualizar los archivos `application.yml` y `application-production.yml` en cada servicio con las credenciales de su base de datos y servidor de correo.

## Documentación de la API

El sistema expone su funcionalidad a través de interfaces REST y GraphQL, documentadas automáticamente para facilitar la integración y el desarrollo.

### Swagger UI (REST)
Cada microservicio cuenta con una interfaz Swagger para explorar y probar los endpoints RESTful.

* **Disponibilidad:** Habilitado en todos los servicios que exponen controladores REST.

### GraphiQL (GraphQL)
Para las consultas y mutaciones de datos, se incluye la interfaz interactiva GraphiQL.

* **Servicios habilitados:**
    * `auth-service`: Gestión de usuarios y roles.
    * `supplier-service`: Consultas de proveedores y productos.
    * `notification-service`: Estado de notificaciones.

---

## Estructura del Proyecto

El repositorio está organizado como un monorepositorio modular, facilitando la gestión centralizada del código y la infraestructura.

```text
pet-manager/
├── api-gateway/          # Puerta de enlace (Gateway): Enrutamiento, seguridad perimetral y CORS.
├── auth-service/         # Servicio de Identidad: Autenticación JWT, gestión de usuarios y seguridad.
├── supplier-service/     # Core de Negocio: Gestión de proveedores, catálogo de productos y condiciones.
├── notification-service/ # Motor de Mensajería: Orquestación de correos y tareas programadas (Schedulers).
├── scripts/              # Infraestructura de Datos: Scripts SQL de inicialización (init-db) y migraciones.
├── docker-compose.yml    # (Opcional) Orquestación de contenedores para despliegue local.
└── README.md             # Documentación general del proyecto.
