# 🧑‍💻 Formulario CRUD de Personas

Aplicación de escritorio desarrollada en **Java** utilizando **Swing**, **JDBC** y **PostgreSQL**, implementando una arquitectura básica basada en el patrón **MVC (Model-View-Controller)**.

El proyecto permite registrar personas mediante un formulario gráfico y almacenar la información en una base de datos PostgreSQL.

> **Estado del proyecto:** En desarrollo
> **Versión:** 1.0-SNAPSHOT

---

## 📋 Descripción

Este proyecto es un ejercicio práctico orientado al aprendizaje de:

* Programación orientada a objetos con Java.
* Desarrollo de interfaces gráficas utilizando Swing.
* Separación de responsabilidades mediante el patrón MVC.
* Conexión de aplicaciones Java con bases de datos mediante JDBC.
* Ejecución de sentencias SQL parametrizadas.
* Gestión de dependencias utilizando Maven.
* Persistencia de información utilizando PostgreSQL.

La aplicación presenta un formulario donde el usuario puede introducir el nombre y apellido de una persona y posteriormente guardar la información en la base de datos.

---

## ✨ Funcionalidades

### Actualmente implementadas

* [x] Ventana gráfica desarrollada con Java Swing.
* [x] Campo para introducir el nombre.
* [x] Campo para introducir el apellido.
* [x] Botón para guardar información.
* [x] Conexión con PostgreSQL mediante JDBC.
* [x] Inserción de registros en la tabla `personas`.
* [x] Limpieza automática del formulario después del registro.
* [x] Separación básica mediante patrón MVC.

### Funcionalidades pendientes

* [ ] Consultar personas registradas.
* [ ] Mostrar personas en una tabla.
* [ ] Actualizar registros.
* [ ] Eliminar registros.
* [ ] Validar campos obligatorios.
* [ ] Mostrar mensajes gráficos de éxito/error.
* [ ] Manejar correctamente el cierre de la conexión.
* [ ] Separar las credenciales de la base de datos del código fuente.
* [ ] Implementar manejo de excepciones más robusto.

---

## 🏗️ Arquitectura

El proyecto utiliza una implementación básica del patrón **MVC**:

```text
┌──────────────────────┐
│        Vista         │
│ PersonFormView       │
│      (Swing)         │
└──────────┬───────────┘
           │
           │ eventos
           ▼
┌──────────────────────┐
│     Controlador      │
│ PersonFormController │
└──────────┬───────────┘
           │
           │ operaciones
           ▼
┌──────────────────────┐
│       Modelo         │
│     PersonModel      │
└──────────┬───────────┘
           │
           │ JDBC / SQL
           ▼
┌──────────────────────┐
│      PostgreSQL      │
│ registro_personas    │
└──────────────────────┘
```

### Modelo

`PersonModel`

Responsable de la comunicación con la base de datos.

Actualmente implementa:

```java
addPerson(String firstName, String lastName)
```

Este método ejecuta una sentencia `INSERT` mediante `PreparedStatement`.

### Vista

`PersonFormView`

Responsable de la interfaz gráfica desarrollada con Swing.

Contiene:

* `JTextField` para nombre.
* `JTextField` para apellido.
* `JButton` para guardar.
* Métodos para obtener los datos introducidos.
* Método para limpiar el formulario.
* Método para registrar el listener del botón.

### Controlador

`PersonFormController`

Actúa como intermediario entre la vista y el modelo.

Recibe el evento producido al pulsar el botón **Guardar**, obtiene los datos de la vista y solicita al modelo que los almacene.

---

## 📁 Estructura del proyecto

```text
ej_formularioCRUD/
│
├── pom.xml
├── .gitignore
│
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   ├── Main.java
│   │   │   │
│   │   │   ├── Controlador/
│   │   │   │   └── PersonFormController.java
│   │   │   │
│   │   │   ├── Modelo/
│   │   │   │   └── PersonModel.java
│   │   │   │
│   │   │   └── Vista/
│   │   │       └── PersonFormView.java
│   │   │
│   │   └── resources/
│   │
│   └── test/
│       └── java/
│
└── target/
```

---

## 🛠️ Tecnologías utilizadas

### Java 22

Lenguaje principal utilizado para desarrollar la aplicación.

### Java Swing

Framework incluido en Java utilizado para construir la interfaz gráfica de escritorio.

### JDBC

API utilizada para establecer la comunicación entre Java y PostgreSQL.

### PostgreSQL

Sistema gestor de bases de datos utilizado para almacenar los registros.

### Maven

Herramienta utilizada para gestionar la configuración del proyecto y sus dependencias.

### PostgreSQL JDBC Driver

Dependencia utilizada para permitir que Java se comunique con PostgreSQL.

Versión utilizada:

```text
42.7.2
```

---

## 🗄️ Base de datos

La aplicación espera una base de datos PostgreSQL llamada:

```text
registro_personas
```

La tabla utilizada por el proyecto es:

```sql
personas
```

El código espera que la tabla contenga al menos las columnas:

```sql
nombre
apellido
```

Un esquema inicial podría ser:

```sql
CREATE DATABASE registro_personas;

CREATE TABLE personas (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    apellido VARCHAR(100) NOT NULL
);
```

---

## 🔌 Configuración de conexión

Actualmente la conexión se encuentra definida directamente en `PersonModel`:

```java
connection = DriverManager.getConnection(
    "jdbc:postgresql://localhost:5432/registro_personas",
    "postgres",
    "tu_contraseña"
);
```

Antes de ejecutar el proyecto es necesario configurar:

```text
Host: localhost
Puerto: 5432
Base de datos: registro_personas
Usuario: postgres
Contraseña: tu_contraseña
```

> ⚠️ Para un proyecto real o de portafolio, las credenciales no deberían almacenarse directamente en el código fuente.

---

## ▶️ Ejecución

### 1. Requisitos

Instalar:

* JDK 22 o compatible.
* PostgreSQL.
* Maven.
* IDE compatible con Java, por ejemplo IntelliJ IDEA.

### 2. Crear la base de datos

Crear la base de datos:

```sql
CREATE DATABASE registro_personas;
```

Después crear la tabla:

```sql
CREATE TABLE personas (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    apellido VARCHAR(100) NOT NULL
);
```

### 3. Configurar las credenciales

Modificar la información de conexión de `PersonModel`:

```java
DriverManager.getConnection(
    "jdbc:postgresql://localhost:5432/registro_personas",
    "postgres",
    "tu_contraseña"
);
```

### 4. Compilar el proyecto

Desde la raíz:

```bash
mvn clean compile
```

### 5. Ejecutar

Ejecutar la clase:

```text
Main.java
```

---

## 🔄 Flujo de funcionamiento

```text
Usuario
   │
   │ introduce nombre y apellido
   ▼
PersonFormView
   │
   │ pulsa "Guardar"
   ▼
PersonFormController
   │
   │ obtiene datos
   ▼
PersonModel
   │
   │ PreparedStatement
   ▼
PostgreSQL
   │
   │ INSERT
   ▼
Registro almacenado
   │
   ▼
Formulario limpiado
```

---

## 🔐 Uso de PreparedStatement

La aplicación utiliza `PreparedStatement`:

```java
String query =
    "INSERT INTO personas (nombre, apellido) VALUES (?, ?)";

PreparedStatement statement =
    connection.prepareStatement(query);

statement.setString(1, firstName);
statement.setString(2, lastName);
```

Esto permite utilizar parámetros en lugar de concatenar directamente los valores proporcionados por el usuario dentro de la sentencia SQL.

---

## 🧪 Pruebas

Actualmente el proyecto contiene la estructura para pruebas:

```text
src/test/java/
```

Sin embargo, no se encuentran pruebas unitarias implementadas en la versión analizada.

Como siguiente etapa se recomienda implementar pruebas para:

* Validación de nombre.
* Validación de apellido.
* Inserción de personas.
* Errores de conexión.
* Operaciones CRUD.
* Comportamiento del controlador.

---

## 🚀 Roadmap

### Versión 1.0

* [x] Crear proyecto Maven.
* [x] Configurar Java.
* [x] Configurar PostgreSQL JDBC.
* [x] Implementar modelo.
* [x] Implementar vista Swing.
* [x] Implementar controlador.
* [x] Registrar personas.

### Versión 2.0

* [ ] Implementar Read.
* [ ] Mostrar registros en `JTable`.
* [ ] Implementar Update.
* [ ] Implementar Delete.
* [ ] Completar CRUD.

### Versión 3.0

* [ ] Validaciones.
* [ ] Manejo de errores mediante diálogos Swing.
* [ ] Separar configuración de base de datos.
* [ ] Mejorar arquitectura.
* [ ] Implementar DAO.
* [ ] Implementar pruebas unitarias.
* [ ] Documentar casos de uso.

---

## 📚 Objetivos de aprendizaje

Este proyecto permite practicar conceptos fundamentales de desarrollo de software:

* Java.
* Programación orientada a objetos.
* MVC.
* Interfaces gráficas.
* Eventos en Swing.
* JDBC.
* SQL.
* PostgreSQL.
* Maven.
* Separación de responsabilidades.
* Persistencia de datos.
* Manejo de excepciones.

---

## 👨‍💻 Autor

Proyecto desarrollado como ejercicio académico y de aprendizaje para fortalecer conocimientos en desarrollo de aplicaciones Java y persistencia de datos.

---

## 📄 Licencia

Este proyecto puede utilizarse con fines educativos y de aprendizaje.

