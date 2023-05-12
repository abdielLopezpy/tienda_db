
# Proyecto de Base de Datos para un Negocio de Venta de Productos en SQL Server

## Descripción

<p>Este proyecto consiste en el diseño de una base de datos para un negocio de venta de productos.<br>
El negocio cuenta con una tienda física y una tienda en línea. <br>
La tienda física cuenta con un sistema de punto de venta, mientras que la tienda en línea cuenta con un sistema de <br>
comercio electrónico. La base de datos esta en Sql Server</p>


<img src="ultimadb.png" alt="Diagrama de la base de datos" caption="Diagrama de la base de datos">


### creacion de la base de datos

```sql
-- Creación de la base de datos
CREATE DATABASE tienda;
```


### Tabla `base_usuario`

| Columna        | Tipo         | Restriccion | Descripción                               | 
|----------------|--------------|-------------|-------------------------------------------|
| id_usuario     | int          | PK          | Llave primaria, identificador del usuario |
| nombre         | varchar(50)  | NOT NULL    | Nombre del usuario                        |
| correo         | varchar(100) | NOT NULL    | Correo electrónico del usuario            |
| contrasena     | varchar(50)  | NOT NULL    | Contraseña del usuario                    |
| fecha_registro | date         | NOT NULL    | Fecha de registro del usuario             |

#### Detalle de la tabla `base_usuario`

<p>La tabla `base_usuario` contiene la información de los usuarios registrations en el sistema. Esta tabla es la base <br>
para las tablas `cliente` y `empleado`, las cuales contienen información adicional de los usuarios.?</p>


### Creación de la tabla `base_usuario`

```sql
-- Tabla usuario
CREATE TABLE base_usuario ( 
    id_usuario INT IDENTITY(1,1) PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL, 
    correo VARCHAR(100) NOT NULL, 
    contrasena VARCHAR(50) NOT NULL,
     fecha_registro DATE NOT NULL);
```

### Ejemplo de inserción de datos en la tabla `base_usuario`

```sql
-- Inserción de datos en la tabla base_usuario
INSERT INTO base_usuario (nombre, correo, contrasena, fecha_registro) VALUES
('Juan Perez', 'jp@mail.com', '123456', '2021-01-01'),
('Maria Lopez', 'ml@mail.com', '123456', '2021-01-01');
```


### Tabla `cliente`

| Columna          | Tipo         | Restriccion | Descripción                                      |
|------------------|--------------|-------------|--------------------------------------------------|
| id_cliente       | int          | PK          | Llave primaria, identificador del cliente        |
| id_usuario       | int          | FK          | Llave foránea, identificador del usuario         |
| telefono         | varchar(20)  | NOT NULL    | Teléfono del cliente                             |
| direccion        | varchar(100) | NOT NULL    | Dirección del cliente                            |
| fecha_nacimiento | date         | NOT NULL    | Fecha de nacimiento del cliente                  |
| id_tipo_cliente  | int          | FK          | Llave foránea, identificador del tipo de cliente |

#### Detalle de la tabla `cliente`

<p>La tabla `cliente` contiene la información de los clientes registrados en el sistema. Esta tabla es la base <br>
para las tablas `compra` y `participacion_campana`, las cuales contienen información adicional de los clientes.</p>

### Creación de la tabla `cliente`

```sql
-- Tabla cliente
CREATE TABLE cliente (
    id_cliente INT IDENTITY(1,1) PRIMARY KEY,
    id_usuario INT FOREIGN KEY REFERENCES base_usuario(id_usuario),
    telefono VARCHAR(20) NOT NULL,
    direccion VARCHAR(100) NOT NULL,
    fecha_nacimiento DATE NOT NULL,
    id_tipo_cliente INT FOREIGN KEY REFERENCES tipo_cliente(id_tipo_cliente)
);
```

### Ejemplo de inserción de datos en la tabla `cliente`

```sql
-- Inserción de datos en la tabla cliente
INSERT INTO cliente (id_usuario, telefono, direccion, fecha_nacimiento, id_tipo_cliente) VALUES
(1, '1234567890', 'Calle 1 # 1-1', '1990-01-01', 1),
(2, '1234567890', 'Calle 2 # 2-2', '1990-01-01', 1);
```

### Tabla `empleado`

| Columna            | Tipo        | Restriccion | Descripción                                       |
|--------------------|-------------|-------------|---------------------------------------------------|
| id_empleado        | int         | PK          | Llave primaria, identificador del empleado        |
| id_usuario         | int         | FK          | Llave foránea, identificador del usuario          |
| puesto             | varchar(50) | NOT NULL    | Puesto del empleado                               |
| salario            | float       | NOT NULL    | Salario del empleado                              |
| fecha_contratacion | date        | NOT NULL    | Fecha de contratación del empleado                |
| id_departamento    | int         | FK          | Llave foránea, identificador del departamento     |
| id_tipo_empleado   | int         | FK          | Llave foránea, identificador del tipo de empleado |

#### Detalle de la tabla `empleado`

<p>La tabla `empleado` contiene la información de los empleados registrados en el sistema.</p>

### Creación de la tabla `empleado`

```sql
-- Tabla empleado
CREATE TABLE empleado (
    id_empleado INT IDENTITY(1,1) PRIMARY KEY,
    id_usuario INT FOREIGN KEY REFERENCES base_usuario(id_usuario),
    puesto VARCHAR(50) NOT NULL,
    salario FLOAT NOT NULL,
    fecha_contratacion DATE NOT NULL,
    id_departamento INT FOREIGN KEY REFERENCES departamento(id_departamento),
    id_tipo_empleado INT FOREIGN KEY REFERENCES tipo_empleado(id_tipo_empleado)
);
```

### Ejemplo de inserción de datos en la tabla `empleado`

```sql
-- Inserción de datos en la tabla empleado

INSERT INTO empleado (id_usuario, puesto, salario, fecha_contratacion, id_departamento, id_tipo_empleado) VALUES
(3, 'Gerente', 1000000, '2021-01-01', 1, 1),
(4, 'Vendedor', 1000000, '2021-01-01', 1, 2);
```


### Tabla `categoria`

| Columna      | Tipo         | Restriccion | Descripción                                   |
|--------------|--------------|-------------|-----------------------------------------------|
| id_categoria | int          | PK          | Llave primaria, identificador de la categoría |    
| nombre       | varchar(50)  | NOT NULL    | Nombre de la categoría                        |
| descripcion  | varchar(100) | NOT NULL    | Descripción de la categoría                   |

#### Detalle de la tabla `categoria`

<p>La tabla `categoria` contiene la información de las categorías de los productos.</p>

### Creación de la tabla `categoria`

```sql
-- Tabla categoria
CREATE TABLE categoria (
    id_categoria INT IDENTITY(1,1) PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL,
    descripcion VARCHAR(100)
);
```

### Ejemplo de inserción de datos en la tabla `categoria`

```sql
-- Inserción de datos en la tabla categoria
INSERT INTO categoria (nombre, descripcion) VALUES
('Categoria 1', 'Descripción de la categoría 1'),
('Categoria 2', 'Descripción de la categoría 2');
```

### Tabla `producto`

| Columna      | Tipo         | Restriccion | Descripción                                  |
|--------------|--------------|-------------|----------------------------------------------|
| id_producto  | int          | PK          | Llave primaria, identificador del producto   |
| id_categoria | int          | FK          | Llave foránea, identificador de la categoría |
| nombre       | varchar(50)  | NOT NULL    | Nombre del producto                          |
| descripcion  | varchar(100) | NOT NULL    | Descripción del producto                     |
| precio       | float        | NOT NULL    | Precio del producto                          |
| existencia   | int          | NOT NULL    | Existencia del producto                      |

#### Detalle de la tabla `producto`

<p>La tabla `producto` contiene la información de los productos registrados en el sistema.</p>

### Creación de la tabla `producto`

```sql
-- Tabla producto
CREATE TABLE producto (
    id_producto INT IDENTITY(1,1) PRIMARY KEY,
    id_categoria INT FOREIGN KEY REFERENCES categoria(id_categoria),
    nombre VARCHAR(50) NOT NULL,
    descripcion VARCHAR(100),
    precio FLOAT NOT NULL,
    existencia INT NOT NULL
);
```

### Ejemplo de inserción de datos en la tabla `producto`

```sql
-- Inserción de datos en la tabla producto
INSERT INTO producto (id_categoria, nombre, descripcion, precio, existencia) VALUES
(1, 'Producto 1', 'Descripción del producto 1', 10000, 10),
(2, 'Producto 2', 'Descripción del producto 2', 20000, 20);
```

### Tabla `promocion`

| Columna         | Tipo         | Restriccion | Descripción                                      |
|-----------------|--------------|-------------|--------------------------------------------------|
| id_promocion    | int          | PK          | Llave primaria, identificador de la promoción    |
| id_tipo_cliente | int          | FK          | Llave foránea, identificador del tipo de cliente |
| nombre          | varchar(50)  | NOT NULL    | Nombre de la promoción                           |
| descripcion     | varchar(100) | NOT NULL    | Descripción de la promoción                      |
| descuento       | float        | NOT NULL    | Descuento de la promoción                        |
| id_tipo_cliente | int          | FK          | Llave foránea, identificador del tipo de cliente |

#### Detalle de la tabla `promocion`

<p>La tabla `promocion` contiene la información de las promociones registradas en el sistema.</p>

### Creación de la tabla `promocion`

```sql
-- Tabla promocion
CREATE TABLE promocion (
    id_promocion INT IDENTITY(1,1) PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL,
    descripcion VARCHAR(100),
    descuento FLOAT NOT NULL,
    id_tipo_cliente INT FOREIGN KEY REFERENCES tipo_cliente(id_tipo_cliente)
);
```

### Ejemplo de inserción de datos en la tabla `promocion`

```sql
-- Inserción de datos en la tabla promocion
INSERT INTO promocion (nombre, descripcion, descuento, id_tipo_cliente) VALUES
('Promoción 1', 'Descripción de la promoción 1', 0.1, 1),
('Promoción 2', 'Descripción de la promoción 2', 0.2, 2);
```

### Tabla `compra`

| Columna      | Tipo         | Restriccion | Descripción                                  |
|--------------|--------------|-------------|----------------------------------------------|
| id_compra    | int          | PK          | Llave primaria, identificador de la compra   |
| id_cliente   | int          | FK          | Llave foránea, identificador del cliente     |
| fecha_compra | date         | NOT NULL    | Fecha de la compra                           |
| total        | float        | NOT NULL    | Total de la compra                           |
| detalles     | varchar(MAX) | NOT NULL    | Detalles de la compra                        |
| id_promocion | int          | FK          | Llave foránea, identificador de la promoción |

#### Detalle de la tabla `compra`

<p>La tabla `compra` contiene la información de las compras registradas en el sistema.</p>

### Creación de la tabla `compra`

```sql

-- Tabla compra
CREATE TABLE compra (
    id_compra INT IDENTITY(1,1) PRIMARY KEY,
    id_cliente INT FOREIGN KEY REFERENCES cliente(id_cliente),
    fecha_compra DATE NOT NULL,
    total FLOAT NOT NULL,
    detalles VARCHAR(MAX),
    id_promocion INT FOREIGN KEY REFERENCES promocion(id_promocion)
);
```

### Ejemplo de inserción de datos en la tabla `compra`

```sql
-- Inserción de datos en la tabla compra
INSERT INTO compra (id_cliente, fecha_compra, total, detalles, id_promocion) VALUES
(1, '2021-01-01', 10000, 'Detalles de la compra 1', 1),
(2, '2021-01-02', 20000, 'Detalles de la compra 2', 2);
```

### Tabla `detalle_compra`

| Columna     | Tipo  | Restriccion | Descripción                               |
|-------------|-------|-------------|-------------------------------------------|
| id_compra   | int   | FK          | Llave foránea, identificador de la compra |
| id_producto | int   | FK          | Llave foránea, identificador del producto |
| cantidad    | int   | NOT NULL    | Cantidad del producto                     |
| precio      | float | NOT NULL    | Precio del producto                       |

#### Detalle de la tabla `detalle_compra`

<p>La tabla `detalle_compra` contiene la información de los detalles de las compras registradas en el sistema.</p>

### Creación de la tabla `detalle_compra`

```sql
-- Tabla detalle_compra
CREATE TABLE detalle_compra (
    id_compra INT FOREIGN KEY REFERENCES compra(id_compra),
    id_producto INT FOREIGN KEY REFERENCES producto(id_producto),
    cantidad INT NOT NULL,
    precio FLOAT NOT NULL
);
```

### Ejemplo de inserción de datos en la tabla `detalle_compra`

```sql
-- Inserción de datos en la tabla detalle_compra
INSERT INTO detalle_compra (id_compra, id_producto, cantidad, precio) VALUES
(1, 1, 1, 10000),
(2, 2, 2, 20000);
```

### Tabla `campana`

| Columna      | Tipo         | Restriccion | Descripción                                 |
|--------------|--------------|-------------|---------------------------------------------|
| id_campana   | int          | PK          | Llave primaria, identificador de la campaña |
| nombre       | varchar(50)  | NOT NULL    | Nombre de la campaña                        |
| descripcion  | varchar(100) | NOT NULL    | Descripción de la campaña                   |
| fecha_inicio | date         | NOT NULL    | Fecha de inicio de la campaña               |
| fecha_fin    | date         | NOT NULL    | Fecha de fin de la campaña                  |

#### Detalle de la tabla `campana`

<p>La tabla `campana` contiene la información de las campañas registradas en el sistema.</p>

### Creación de la tabla `campana`

```sql

-- Tabla campana
CREATE TABLE campana (
    id_campana INT IDENTITY(1,1) PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL,
    descripcion VARCHAR(100),
    fecha_inicio DATE NOT NULL,
    fecha_fin DATE NOT NULL
);
```

### Ejemplo de inserción de datos en la tabla `campana`

```sql
-- Inserción de datos en la tabla campana
INSERT INTO campana (nombre, descripcion, fecha_inicio, fecha_fin) VALUES
('Campaña 1', 'Descripción de la campaña 1', '2021-01-01', '2021-01-31'),
('Campaña 2', 'Descripción de la campaña 2', '2021-02-01', '2021-02-28');
```

### Tabla `participacion_campana`

| Columna          | Tipo | Restriccion | Descripción                                       |
|------------------|------|-------------|---------------------------------------------------|
| id_participacion | int  | PK          | Llave primaria, identificador de la participación |
| id_empleado      | int  | FK          | Llave foránea, identificador del empleado         |
| id_cliente       | int  | FK          | Llave foránea, identificador del cliente          |
| id_campana       | int  | FK          | Llave foránea, identificador de la campaña        |

#### Detalle de la tabla `participacion_campana`

<p>La tabla `participacion_campana` contiene la información de las participaciones de los empleados y <br>
clientes en las campañas registradas en el sistema.</p>

### Creación de la tabla `participacion_campana`

```sql

-- Tabla participacion_campana
CREATE TABLE participacion_campana (
    id_participacion INT IDENTITY(1,1) PRIMARY KEY,
    id_empleado INT FOREIGN KEY REFERENCES empleado(id_empleado),
    id_cliente INT FOREIGN KEY REFERENCES cliente(id_cliente),
    id_campana INT FOREIGN KEY REFERENCES campana(id_campana)
);
```

### Ejemplo de inserción de datos en la tabla `participacion_campana`

```sql
-- Inserción de datos en la tabla participacion_campana
INSERT INTO participacion_campana (id_empleado, id_cliente, id_campana) VALUES
(1, 1, 1),
(2, 2, 2);
```

### Tabla `envio`

| Columna                | Tipo         | Restriccion | Descripción                               |
|------------------------|--------------|-------------|-------------------------------------------|
| id_envio               | int          | PK          | Llave primaria, identificador del envío   |
| id_compra              | int          | FK          | Llave foránea, identificador de la compra |
| fecha_envio            | date         | NOT NULL    | Fecha de envío                            |
| fecha_estimada_entrega | date         | NOT NULL    | Fecha estimada de entrega                 |
| estado                 | varchar(50)  | NOT NULL    | Estado del envío                          |
| direccion_envio        | varchar(100) | NOT NULL    | Dirección de envío                        |
| metodo_envio           | varchar(50)  | NOT NULL    | Método de envío                           |
| id_empleado            | int          | FK          | Llave foránea, identificador del empleado |

#### Detalle de la tabla `envio`

<p>La tabla `envio` contiene la información de los envíos registrados en el sistema.</p>

### Creación de la tabla `envio`

```sql
-- Tabla envio
CREATE TABLE envio (
    id_envio INT IDENTITY(1,1) PRIMARY KEY,
    id_compra INT FOREIGN KEY REFERENCES compra(id_compra),
    fecha_envio DATE NOT NULL,
    fecha_estimada_entrega DATE NOT NULL,
    estado VARCHAR(50) NOT NULL,
    direccion_envio VARCHAR(100) NOT NULL,
    metodo_envio VARCHAR(50) NOT NULL,
    id_empleado INT FOREIGN KEY REFERENCES empleado(id_empleado)
);

-- Restricción de clave foránea en la tabla envio
ALTER TABLE envio WITH CHECK ADD CONSTRAINT FK_empleado_envio FOREIGN KEY (id_empleado) REFERENCES empleado(id_empleado);
```

### Ejemplo de inserción de datos en la tabla `envio`

```sql
-- Inserción de datos en la tabla envio
INSERT INTO envio (id_compra, fecha_envio, fecha_estimada_entrega, estado, direccion_envio, metodo_envio, id_empleado) VALUES
(1, '2021-01-01', '2021-01-05', 'Enviado', 'Dirección de envío 1', 'Método de envío 1', 1),
(2, '2021-02-01', '2021-02-05', 'Enviado', 'Dirección de envío 2', 'Método de envío 2', 2);
```

### Tabla `factura_electronica`

| Columna            | Tipo         | Restriccion | Descripción                                 |
|--------------------|--------------|-------------|---------------------------------------------|
| id_factura         | int          | PK          | Llave primaria, identificador de la factura |
| id_compra          | int          | FK          | Llave foránea, identificador de la compra   |
| fecha_emision      | datetime     | NOT NULL    | Fecha de emisión de la factura              |
| subtotal           | float        | NOT NULL    | Subtotal de la factura                      |
| impuestos          | float        | NOT NULL    | Impuestos de la factura                     |
| total              | float        | NOT NULL    | Total de la factura                         |
| descripcion        | varchar(100) | NOT NULL    | Descripción de la factura                   |
| estado             | varchar(50)  | NOT NULL    | Estado de la factura                        |
| numero_serie       | varchar(20)  | NOT NULL    | Número de serie de la factura               |
| numero_correlativo | int          | NOT NULL    | Número correlativo de la factura            |

#### Detalle de la tabla `factura_electronica`

<p>La tabla `factura_electronica` contiene la información de las facturas registradas en el sistema.</p>

### Creación de la tabla `factura_electronica`

```sql
-- Tabla factura_electronica
CREATE TABLE factura_electronica (
    id_factura INT IDENTITY(1,1) PRIMARY KEY,
    id_compra INT FOREIGN KEY REFERENCES compra(id_compra),
    fecha_emision DATETIME NOT NULL,
    subtotal FLOAT NOT NULL,
    impuestos FLOAT NOT NULL,
    total FLOAT NOT NULL,
    descripcion VARCHAR(100),
    estado VARCHAR(50) NOT NULL,
    numero_serie VARCHAR(20) NOT NULL,
    numero_correlativo INT NOT NULL
);
```

### Ejemplo de inserción de datos en la tabla `factura_electronica`

```sql
-- Inserción de datos en la tabla factura_electronica   
INSERT INTO factura_electronica (id_compra, fecha_emision, subtotal, impuestos, total, descripcion, estado, numero_serie, numero_correlativo) VALUES
(1, '2021-01-01 00:00:00', 100.00, 18.00, 118.00, 'Descripción de la factura 1', 'Emitida', 'Serie 1', 1),
(2, '2021-02-01 00:00:00', 200.00, 36.00, 236.00, 'Descripción de la factura 2', 'Emitida', 'Serie 2', 2);
```

### Tabla `departamento`

| Columna         | Tipo        | Restriccion | Descripción                                    |
|-----------------|-------------|-------------|------------------------------------------------|
| id_departamento | int         | PK          | Llave primaria, identificador del departamento |
| nombre          | varchar(50) | NOT NULL    | Nombre del departamento                        |

#### Detalle de la tabla `departamento`

<p>La tabla `departamento` contiene la información de los departamentos registrados en el sistema.</p>

### Creación de la tabla `departamento`

```sql
-- Tabla departamento
CREATE TABLE departamento (
    id_departamento INT IDENTITY(1,1) PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL
);
```

### Ejemplo de inserción de datos en la tabla `departamento`

```sql
-- Inserción de datos en la tabla departamento
INSERT INTO departamento (nombre) VALUES
('Departamento 1'),
('Departamento 2');
```

### Tabla `tipo_empleado`

| Columna          | Tipo        | Restriccion | Descripción                                        |
|------------------|-------------|-------------|----------------------------------------------------|
| id_tipo_empleado | int         | PK          | Llave primaria, identificador del tipo de empleado |
| nombre           | varchar(50) | NOT NULL    | Nombre del tipo de empleado                        |

#### Detalle de la tabla `tipo_empleado`

<p>La tabla `tipo_empleado` contiene la información de los tipos de empleados registrados en el sistema.</p>

### Creación de la tabla `tipo_empleado`

```sql
-- Tabla tipo_empleado
CREATE TABLE tipo_empleado (
    id_tipo_empleado INT IDENTITY(1,1) PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL
);
```

### Ejemplo de inserción de datos en la tabla `tipo_empleado`

```sql
-- Inserción de datos en la tabla tipo_empleado
INSERT INTO tipo_empleado (nombre) VALUES
('Tipo de empleado 1'),
('Tipo de empleado 2');
```

### Tabla `tipo_cliente`

| Columna         | Tipo        | Restriccion | Descripción                                       |
|-----------------|-------------|-------------|---------------------------------------------------|
| id_tipo_cliente | int         | PK          | Llave primaria, identificador del tipo de cliente |
| nombre          | varchar(50) | NOT NULL    | Nombre del tipo de cliente                        |

#### Detalle de la tabla `tipo_cliente`

<p>La tabla `tipo_cliente` contiene la información de los tipos de clientes registrados en el sistema.</p>

### Creación de la tabla `tipo_cliente`

```sql

-- Tabla tipo_cliente
CREATE TABLE tipo_cliente (
    id_tipo_cliente INT IDENTITY(1,1) PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL
);
```

### Ejemplo de inserción de datos en la tabla `tipo_cliente`

```sql
-- Inserción de datos en la tabla tipo_cliente
INSERT INTO tipo_cliente (nombre) VALUES
('Tipo de cliente 1'),
('Tipo de cliente 2');
```




