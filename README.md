## Descripción General

Este proyecto consiste en una **API REST desarrollada con Node.js, Express y MongoDB**, siguiendo los principios de **Arquitectura Limpia (Clean Architecture)**. El sistema permite la gestión de órdenes de productos de computadoras, incluyendo autenticación de usuarios, manejo de roles y control de acceso mediante JWT.

El proyecto fue desarrollado como parte de un **Diplomado en Programación**, con el objetivo de aplicar buenas prácticas de desarrollo backend y separación de responsabilidades.

---

## Tecnologías Utilizadas

* Node.js
* Express.js
* MongoDB
* Mongoose
* JSON Web Token (JWT)
* Bcrypt
* Swagger (Documentación de API)
* Morgan

---

## Arquitectura del Proyecto

El proyecto sigue el enfoque de **Arquitectura Limpia**, dividiendo la aplicación en capas bien definidas:

* **Domain:** Contiene las entidades y reglas de negocio.
* **Application:** Casos de uso y lógica de aplicación.
* **Infrastructure:** Acceso a base de datos y configuraciones externas.
* **Presentation:** Controladores, rutas y middlewares.

Esta separación permite que el sistema sea escalable, mantenible y fácil de testear.

---

## Documentación de Entidades

###  USER

**Descripción:**
La entidad User representa a los usuarios del sistema que pueden autenticarse y realizar operaciones según el rol asignado.

**Campos principales:**

| Campo     | Tipo     | Descripción              |
| --------- | -------- | ------------------------ |
| name      | String   | Nombre del usuario       |
| email     | String   | Correo electrónico único |
| password  | String   | Contraseña encriptada    |
| role      | ObjectId | Rol asignado al usuario  |
| createdAt | Date     | Fecha de creación        |

**Operaciones:**

* Crear usuario
* Listar usuarios
* Obtener usuario por ID
* Actualizar usuario
* Eliminar usuario

**Relaciones:**

* Un usuario tiene un rol
* Un usuario puede crear órdenes

---

###  ROLES

**Descripción:**
La entidad Role define los niveles de acceso y permisos dentro del sistema.

**Campos principales:**

| Campo       | Tipo   | Descripción               |
| ----------- | ------ | ------------------------- |
| name        | String | Nombre del rol            |
| permissions | Array  | Permisos asociados al rol |
| createdAt   | Date   | Fecha de creación         |

**Ejemplos de roles:**

* ADMIN
* USER

---

###  PRODUCT

**Descripción:**
La entidad Product representa los productos de computadoras disponibles para ser utilizados en las órdenes.

**Campos principales:**

| Campo       | Tipo   | Descripción              |
| ----------- | ------ | ------------------------ |
| name        | String | Nombre del producto      |
| description | String | Descripción del producto |
| price       | Number | Precio unitario          |
| stock       | Number | Cantidad disponible      |
| createdAt   | Date   | Fecha de registro        |

**Relaciones:**

* Un producto puede estar asociado a varias órdenes

---

###  AUTH

**Descripción:**
La entidad Auth gestiona la autenticación y autorización del sistema mediante el uso de tokens JWT.

**Funcionalidades principales:**

* Autenticación de usuarios
* Generación de token JWT
* Validación de token
* Protección de rutas privadas

**Flujo de autenticación:**

1. El usuario envía sus credenciales (email y password)
2. El sistema valida la información
3. Se genera un token JWT
4. El token permite el acceso a rutas protegidas

---

## CRUD de la Entidad Order

La entidad Order permite la gestión completa de órdenes de productos.

**Campos principales:**

| Campo       | Tipo     | Descripción                     |
| ----------- | -------- | ------------------------------- |
| product     | ObjectId | Producto asociado               |
| description | String   | Descripción del producto        |
| quantity    | Number   | Cantidad solicitada             |
| price       | Number   | Precio unitario                 |
| discount    | Number   | Descuento aplicado              |
| total       | Number   | Total calculado automáticamente |
| status      | String   | Estado de la orden              |
| createdAt   | Date     | Fecha de creación               |

**Operaciones CRUD:**

* Crear orden
* Listar órdenes
* Obtener orden por ID
* Actualizar orden
* Eliminar orden

El campo **total** se calcula automáticamente en base a la cantidad, precio y descuento.

---

## Ejecución del Proyecto

1. Clonar el repositorio:

```bash
git clone <url-del-repositorio>
```

2. Instalar dependencias:

```bash
npm install
```

3. Configurar variables de entorno:
   Crear un archivo `.env` basado en `.env.example`

4. Ejecutar el proyecto:

```bash
npm run dev
```

---

## Documentación de la API

La API cuenta con documentación interactiva mediante **Swagger**, la cual permite probar los endpoints disponibles.

---

## Entidad Cupon ##

La entidad **Cupon** permite gestionar cupones de descuento que pueden ser utilizados en las órdenes del sistema.  
Incluye control de activación, fecha de expiración y límite de usos, siguiendo los principios de **Arquitectura Limpia**.

---

###  Modelo de Datos

La entidad Cupon cuenta con los siguientes campos:

| Campo | Tipo | Descripción |
|------|------|-------------|
| codigo | String | Código único del cupón (en mayúsculas) |
| descuento | Number | Porcentaje de descuento (0–100) |
| fechaExpiracion | Date | Fecha de vencimiento del cupón |
| activo | Boolean | Indica si el cupón está habilitado |
| usoMaximo | Number | Cantidad máxima de veces que puede usarse |
| usoActual | Number | Cantidad de veces que ya fue usado |
| createdAt | Date | Fecha de creación |
| updatedAt | Date | Fecha de última actualización |

---

###  Arquitectura

La funcionalidad de Cupon está implementada siguiendo **Arquitectura Limpia**, separando responsabilidades en capas:

- **Domain**
  - `CuponRepository` (interfaz)
- **Infrastructure**
  - `CuponMongoRepository` (implementación MongoDB)
- **Application**
  - `CuponService` (casos de uso)
- **Presentation**
  - `CuponController`
  - `cupon.routes`

Esto permite un sistema desacoplado, mantenible y escalable.

---

###  Endpoints de Cupon

> Todos los endpoints están protegidos mediante autenticación JWT y validación de rol administrador cuando corresponde.

---

####  Obtener todos los cupones
- **Método:** GET  
- **Endpoint:** `/api/cupons`

**Descripción:**  
Retorna la lista completa de cupones registrados.

---

####  Obtener cupón por ID
- **Método:** GET  
- **Endpoint:** `/api/cupons/:id`

---

####  Obtener cupón por código
- **Método:** GET  
- **Endpoint:** `/api/cupons/codigo/:codigo`

**Descripción:**  
Busca un cupón específico usando su código.

---

####  Obtener cupones activos
- **Método:** GET  
- **Endpoint:** `/api/cupons/activos`

---

####  Crear cupón
- **Método:** POST  
- **Endpoint:** `/api/cupons`

**Ejemplo de Request:**
```json
{
  "codigo": "DESC20",
  "descuento": 20,
  "fechaExpiracion": "2025-12-31",
  "usoMaximo": 100
}

