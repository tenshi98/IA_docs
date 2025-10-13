# Formas de documentar un proyecto

- La documentación de un proyecto de informática o software se clasifica generalmente en cuatro tipos principales, enfocados a diferentes audiencias y etapas del ciclo de vida del desarrollo:

| Tipo de Documentación | Audiencia Principal | Contenido Resumido |
| --- | --- | --- |
| 1. De Requisitos (Qué se va a construir) | Stakeholders, Analistas, Desarrolladores | Define el alcance y las necesidades. Incluye el Caso de Negocio, la Especificación de Requisitos de Software (ERS), historias de usuario, requisitos funcionales y no funcionales (rendimiento, seguridad, etc.). |
| 2. De Arquitectura y Diseño (Cómo se va a construir) | Desarrolladores, Arquitectos | Proporciona el plano del sistema. Incluye la Arquitectura del Sistema (capas, módulos), modelos de datos (diagramas UML, ER), diseño de interfaces (wireframes, mockups) y decisiones técnicas clave. |
| 3. Técnica (Interna) (Detalles del código) | Desarrolladores, Personal de Mantenimiento | Detalla los aspectos internos del software. Incluye la Documentación de API (con herramientas como Swagger/OpenAPI), comentarios en el código, guías de instalación y despliegue (DevOps), y el registro de decisiones técnicas. |
| 4. De Usuario (Externa) (Cómo usar el producto) | Usuarios Finales, Administradores | Ayuda a los usuarios a interactuar con el sistema. Incluye Manuales de Usuario, tutoriales, guías de instalación, preguntas frecuentes (FAQ), y documentación de soporte para la mesa de ayuda. |

## Elementos clave de la documentación de requisitos

### A continuación, se presenta un resumen estructurado de los elementos clave de la documentación de requisitos (Casos de Uso, Historias de Usuario, y Especificaciones Funcionales/No Funcionales), siguiendo los estándares comunes en proyectos de software.

- A. Casos de Uso (UML - Alto Nivel)

| Caso de Uso | Actor Principal | Resumen |
| --- | --- | --- |
| Gestionar Cuentas | Usuario | Crear, ver, actualizar o eliminar cuentas financieras (ej. Banco A, Efectivo, Tarjeta). |
| Registrar Transacción | Usuario | Registrar un nuevo ingreso o egreso, asociándolo a una cuenta, categoría y fecha. |
| Consultar Historial | Usuario | Buscar y filtrar transacciones por fecha, cuenta o categoría. |
| Gestionar Categorías | Usuario | Crear, editar y eliminar categorías de gastos/ingresos (ej. Comida, Nómina, Alquiler). |
| Generar Reportes | Usuario | Obtener informes de resumen de gastos e ingresos en un período específico. |
| Autenticación | Usuario | Registrarse (crear cuenta) e iniciar/cerrar sesión en la aplicación. |

- B. Historias de Usuario (Funcionalidades Clave)

Las historias de usuario definen la funcionalidad desde la perspectiva del usuario.

| Rol | Necesidad (Función) | Razón (Valor) |
| --- | --- | --- |
| Como Usuario | quiero registrar un nuevo gasto | para mantener un control exacto de dónde se va mi dinero. |
| Como Usuario | quiero filtrar mi historial de egresos por mes y categoría | para identificar rápidamente mis áreas de mayor gasto. |
| Como Usuario | quiero crear una nueva cuenta financiera (ej. Visa Crédito) | para separar y monitorear los saldos de mis diferentes fuentes de dinero. |
| Como Usuario | quiero ver un saldo total consolidado | para saber mi posición financiera actual en un solo lugar. |
| Como Administrador | quiero modificar una transacción ya registrada | si cometí un error al ingresar el monto o la categoría. |

- C. Especificaciones Funcionales (Lo que la API DEBE hacer)

| ID | Módulo | Especificación Funcional |
| --- | --- | --- |
| EF-001 | Transacciones | La API DEBE permitir el envío de una solicitud POST para crear una nueva transacción (ingreso o egreso) con campos obligatorios: monto, fecha, tipo y cuenta_id. |
| EF-002 | Cuentas | La API DEBE devolver una lista de todas las cuentas registradas para el usuario autenticado a través de una solicitud GET /accounts. |
| EF-003 | Autenticación | La API DEBE validar las credenciales de un usuario y, si son correctas, responder con un Token de Acceso (JWT). |
| EF-004 | Reportes | La API DEBE calcular y devolver el balance neto (Ingresos - Egresos) para un rango de fechas dado. |
| EF-005 | Categorías | La API DEBE impedir que se elimine una categoría si está asociada a alguna transacción activa. |

- D. Especificaciones No Funcionales (Cómo debe funcionar la API)

Las especificaciones no funcionales definen los criterios de calidad y restricciones.

| Categoría | Especificación No Funcional | Detalle |
| --- | --- | --- |
| Rendimiento | Latencia de Respuesta | El 95% de las solicitudes a GET /transactions (con filtrado) DEBE ser respondido en menos de 500 ms. |
| Seguridad | Autenticación y Autorización | Todas las rutas (excepto login/registro) DEBEN requerir un token JWT válido (OAuth 2.0/Bearer Token). |
| Seguridad | Protección de Datos | La API DEBE encriptar la información sensible (contraseñas) en la base de datos (ej. con Bcrypt). |
| Disponibilidad | Tiempo de Actividad | La API DEBE garantizar una disponibilidad del 99.9% (Uptime) en horario laboral. |
| Usabilidad (API) | Documentación | La API DEBE contar con documentación interactiva y actualizada (ej. Swagger/OpenAPI) para todos los endpoints. |
| Mantenibilidad | Pruebas Unitarias | El código del backend DEBE alcanzar una cobertura de pruebas unitarias mínima del 80%. |


# 💰 API Administrador de Gastos (Wallet API)

Una API RESTful robusta y segura diseñada para gestionar las finanzas personales: registrar ingresos, egresos, controlar saldos de cuentas y generar reportes de balance en tiempo real.

## 🎯 Requisitos Clave del Proyecto

Esta API fue desarrollada para satisfacer las siguientes necesidades principales:

### 1. Funcionalidades (Historias de Usuario)
* **Gestión de Transacciones:** Permitir al usuario registrar y actualizar gastos e ingresos con su monto, fecha y descripción.
* **Control de Cuentas:** Capacidad para el usuario de crear, nombrar y monitorear diferentes cuentas financieras (ej. 'Efectivo', 'Banco A', 'Tarjeta Crédito').
* **Clasificación Flexible:** Permitir la personalización de categorías para un análisis detallado del flujo de dinero (ej. 'Comida', 'Nómina', 'Alquiler').
* **Generación de Balance:** Calcular el balance neto (Ingresos - Egresos) de manera consolidada o por períodos específicos.

### 2. Especificaciones No Funcionales
| Aspecto | Requisito Mínimo |
| :--- | :--- |
| **Seguridad** | Uso obligatorio de **JWT (JSON Web Tokens)** para todas las rutas protegidas. |
| **Rendimiento** | Latencia de respuesta en consultas de transacciones (con filtros) menor a **500 ms**. |
| **Integridad** | Contraseñas almacenadas en base de datos deben estar **encriptadas (hashed)**. |
| **Documentación** | Debe incluir documentación interactiva de endpoints con **Swagger/OpenAPI**. |

## 🏗️ Diseño y Arquitectura del Sistema

### 1. Arquitectura
La API sigue una arquitectura de **capas (Backend)**, separando la gestión de peticiones, la lógica de negocio y la persistencia de datos.

### 2. Modelo de Datos (Esquema Básico)

Se utiliza un modelo de Entidad-Relación relacional:

* `Usuario (1)` a `Cuenta (N)`
* `Usuario (1)` a `Categoría (N)`
* `Cuenta (N)` a `Transacción (1)`
* `Categoría (N)` a `Transacción (1)`

| Entidad | Campos Clave | Relaciones |
| :--- | :--- | :--- |
| **`Usuario`** | `id`, `email`, `contraseña` (hashed) | FK a `Cuenta`, `Categoría` |
| **`Cuenta`** | `id`, `nombre`, `saldo_actual`, `usuario_id` | FK a `Transacción` |
| **`Transacción`** | `id`, `monto`, `fecha`, `tipo` (`Ingreso`/`Egreso`) | FK a `Cuenta`, `Categoría` |

## 🛠️ Guía de Uso de la API (Endpoints Principales)

A continuación, se detallan los *endpoints* principales de la API. Recuerde que todas las rutas marcadas con **(Auth)** requieren el **Token JWT** en la cabecera `Authorization: Bearer <token>`.

### Autenticación
| Método | Ruta | Descripción |
| :--- | :--- | :--- |
| `POST` | `/auth/register` | Crea una nueva cuenta de usuario. |
| `POST` | `/auth/login` | Inicia sesión y devuelve el token de acceso. |

### Cuentas (Auth)
| Método | Ruta | Descripción |
| :--- | :--- | :--- |
| `GET` | `/accounts` | Lista todas las cuentas del usuario. |
| `POST` | `/accounts` | Crea una nueva cuenta financiera. |
| `PUT` | `/accounts/{id}` | Modifica una cuenta existente. |

### Transacciones (Auth)
| Método | Ruta | Descripción | Ejemplo de Query Params |
| :--- | :--- | :--- | :--- |
| `GET` | `/transactions` | Lista y filtra transacciones. | `?start_date=2024-01-01&category_id=5` |
| `POST` | `/transactions` | Registra un nuevo ingreso o egreso. | Body: `{monto: 100.00, tipo: "Egreso", ...}` |
| `DELETE` | `/transactions/{id}` | Elimina una transacción. | |

### Reportes (Auth)
| Método | Ruta | Descripción | Ejemplo de Query Params |
| :--- | :--- | :--- | :--- |
| `GET` | `/reports/balance` | Retorna el balance neto (Ingresos - Egresos) de un período. | `?start_date=2024-01-01&end_date=2024-03-31` |

## 🚀 Instalación y Despliegue

### Requisitos Previos

* [Node.js](https://nodejs.org/) (v18.x o superior)
* [PostgreSQL](https://www.postgresql.org/) o MySQL

### Pasos de Configuración

1.  **Clonar el Repositorio:**
    ```bash
    git clone [https://github.com/tu-usuario/wallet-api.git](https://github.com/tu-usuario/wallet-api.git)
    cd wallet-api
    ```
2.  **Instalar Dependencias:**
    ```bash
    npm install
    ```
3.  **Variables de Entorno:**
    Crea un archivo `.env` en la raíz del proyecto con la siguiente estructura:
    ```env
    PORT=3000
    DATABASE_URL="postgres://user:password@host:port/dbname"
    JWT_SECRET="una_clave_secreta_fuerte"
    ```
4.  **Ejecutar Migraciones de DB:**
    ```bash
    npm run migrate
    ```
5.  **Iniciar la API:**
    ```bash
    npm start
    ```

La API estará corriendo en `http://localhost:3000`. Consulta la documentación de Swagger para las especificaciones detalladas de cada *endpoint*.








