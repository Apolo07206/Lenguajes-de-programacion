---
title: Conceptos de mysql
area: programacion
tipo: lenguaje
tags: [programacion, mysql]
created: 2026-09-18
updated: 2026-09-20
related: ["[[MOC-programacion]]", "[[Ruta Mysql]]", "[[SQLmap]]"]
---

## 1. DDL (Data Definition Language) - Manejo de Tablas y Base de Datos

### Crear Base de Datos y Tablas

SQL

```
-- Crear base de datos si no existe
CREATE DATABASE IF NOT EXISTS sena_db;
USE sena_db;

-- Crear tabla
CREATE TABLE estudiante (
    id_estudiante INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL,
    apellido VARCHAR(50) NOT NULL,
    telefono VARCHAR(15),
    edad INT,
    nota DECIMAL(3, 1),
    fecha_nacimiento DATE
);

CREATE TABLE libro (
    id_libro INT AUTO_INCREMENT PRIMARY KEY,
    titulo VARCHAR(100) NOT NULL,
    autor VARCHAR(100),
    editorial VARCHAR(50),
    precio DECIMAL(10, 2)
);
```

### Modificar Columnas de una Tabla (`ALTER TABLE`)

SQL

```
-- Agregar una nueva columna
ALTER TABLE estudiante 
ADD COLUMN email VARCHAR(100);

-- Modificar el tipo de dato o restricción de una columna
ALTER TABLE estudiante 
MODIFY COLUMN telefono VARCHAR(20);

-- Renombrar una columna
ALTER TABLE estudiante 
CHANGE COLUMN edad edad_estudiante INT;

-- Eliminar una columna
ALTER TABLE estudiante 
DROP COLUMN email;
```

### Eliminar Tablas y Base de Datos (`DROP`)

SQL

```
-- Eliminar una tabla completa
DROP TABLE IF EXISTS libro;

-- Eliminar la base de datos completa
DROP DATABASE IF EXISTS sena_db;
```

## 2. DML (Data Manipulation Language) - Insertar, Actualizar y Eliminar Datos

### Insertar Datos (`INSERT INTO`)

SQL

```
-- Insertar un solo registro
INSERT INTO estudiante (nombre, apellido, telefono, edad, nota, fecha_nacimiento)
VALUES ('Carlos', 'Arias', '3001234567', 20, 4.5, '2004-05-15');

-- Insertar múltiples registros a la vez
INSERT INTO libro (titulo, autor, editorial, precio) VALUES 
('Moby Dick', 'Herman Melville', 'Planeta', 45000.00),
('Cien Años de Soledad', 'Gabriel García Márquez', 'Sudamericana', 60000.00),
('Metamorfosis', 'Franz Kafka', 'Editorial M', 30000.00);
```

### Actualizar Datos (`UPDATE`)

SQL

```
-- Actualizar la nota de un estudiante específico
UPDATE estudiante 
SET nota = 4.8 
WHERE id_estudiante = 1;

-- Actualizar el valor de todos los libros con un aumento del 5%[cite: 1]
UPDATE libro 
SET precio = precio * 1.05;
```

### Eliminar Datos (`DELETE`)

SQL

```
-- Eliminar un registro específico
DELETE FROM estudiante 
WHERE id_estudiante = 1;

-- Eliminar estudiantes con nota menor a 3.0
DELETE FROM estudiante 
WHERE nota < 3.0;

-- ⚠️ Eliminar TODOS los datos de una tabla (sin borrar la estructura)
TRUNCATE TABLE estudiante;
```

## 3. DQL (Data Query Language) - Consultas y Operadores

### Consultas Básicas y Expresiones Matemáticas

SQL

```
-- Seleccionar todos los campos
SELECT * FROM estudiante;

-- Seleccionar columnas específicas y calcular valor con incremento del 5%[cite: 1]
SELECT titulo, precio, (precio * 1.05) AS precio_con_incremento 
FROM libro;

-- Concatenar columnas separadas por coma[cite: 1]
SELECT CONCAT(nombre, ', ', apellido, ', ', telefono, ', ', edad) AS datos_estudiante 
FROM estudiante;[cite: 1]

SELECT CONCAT(titulo, ', ', editorial, ', ', autor) AS datos_libro 
FROM libro;[cite: 1]
```

### Ordenamiento (`ORDER BY`)

SQL

```
-- Seleccionar y mostrar estudiantes en orden ascendente (por nombre)[cite: 1]
SELECT * FROM estudiante 
ORDER BY nombre ASC;[cite: 1]

-- Ordenar por número de columna (ejemplo: columna 2)[cite: 1]
SELECT * FROM acudiente 
ORDER BY 2;[cite: 1]
```

### Operadores Lógicos y Filtrado (`AND`, `OR`, `BETWEEN`, `IN`, `LIKE`)

#### `AND` / `OR`

SQL

```
-- Buscar estudiantes de más de 18 años Y con nota mayor o igual a 4.0
SELECT * FROM estudiante 
WHERE edad >= 18 AND nota >= 4.0;

-- Buscar libros de la editorial 'Planeta' O 'Sudamericana'
SELECT * FROM libro 
WHERE editorial = 'Planeta' OR editorial = 'Sudamericana';
```

#### `BETWEEN` (Entre un rango de valores)

SQL

```
-- Estudiantes con notas entre 3.0 y 5.0[cite: 1]
SELECT * FROM estudiante 
WHERE nota BETWEEN 3.0 AND 5.0;[cite: 1]

-- Estudiantes nacidos entre los años 2000 y 2010[cite: 1]
SELECT * FROM estudiante 
WHERE fecha_nacimiento BETWEEN '2000-01-01' AND '2010-12-31';[cite: 1]
```

#### `IN` (Dentro de una lista de valores)

SQL

```
-- Filtrar registros que coincidan con cualquiera de los valores especificados
SELECT * FROM libro 
WHERE editorial IN ('Planeta', 'Sudamericana', 'Norma');
```

#### `LIKE` (Búsqueda por patrones con `%`)

SQL

```
-- Libros que comiencen con la letra 'M'[cite: 1]
SELECT * FROM libro 
WHERE titulo LIKE 'M%';[cite: 1]

-- Estudiantes cuyo nombre contenga la subsecuencia "ar"[cite: 1]
SELECT * FROM estudiante 
WHERE nombre LIKE '%ar%';[cite: 1]
```

## 4. Consultas a Múltiples Tablas (`JOIN`) y Alias (`AS`)

Para consultar tablas relacionadas (por ejemplo, relacionar estudiantes con sus préstamos de libros):

SQL

```
-- Crear tabla intermedia de ejemplo para préstamos
CREATE TABLE prestamo (
    id_prestamo INT AUTO_INCREMENT PRIMARY KEY,
    id_estudiante INT,
    id_libro INT,
    fecha_prestamo DATE,
    FOREIGN KEY (id_estudiante) REFERENCES estudiante(id_estudiante),
    FOREIGN KEY (id_libro) REFERENCES libro(id_libro)
);

-- Consulta con INNER JOIN y Apodos/Alias a las tablas
SELECT 
    e.nombre AS nombre_estudiante,
    e.apellido AS apellido_estudiante,
    l.titulo AS titulo_libro,
    p.fecha_prestamo
FROM prestamo AS p
INNER JOIN estudiante AS e ON p.id_estudiante = e.id_estudiante
INNER JOIN libro AS l ON p.id_libro = l.id_libro;
```


### Operadores de Ordenamiento por Índice de Columna

Puedes ordenar el resultado indicando el número de posición de la columna en lugar de su nombre.

SQL

```
-- Seleccionar y mostrar la información de la tabla acudiente por la columna 2 (ej. nombre o apellido)
SELECT * 
FROM acudiente 
ORDER BY 2;[cite: 1]
```

### Expresiones Matemáticas en Consultas (`SELECT` con Cálculo)

Puedes realizar operaciones aritméticas directamente dentro de un `SELECT` sin modificar los datos guardados en la tabla.

SQL

```
-- Seleccionar y mostrar el valor total de los productos con un incremento del 5%[cite: 1]
SELECT 
    id_producto, 
    nombre_producto, 
    precio, 
    (precio * 1.05) AS precio_con_incremento_5pct 
FROM producto;[cite: 1]
```

### Búsqueda de Cadenas por Patrones (`LIKE`)

El operador `LIKE` junto con `%` permite hacer búsquedas flexibles dentro de textos:

- `'M%'`: Texto que **comienza** con la letra M[cite: 1].
    
- `'%'`: Representa cero, uno o varios caracteres.
    

SQL

```
-- Seleccionar y mostrar la información de los libros que comiencen con la letra M[cite: 1]
SELECT * 
FROM libro 
WHERE titulo LIKE 'M%';[cite: 1]
```

### Concatenación y Formato de Salida (`CONCAT`)

La función `CONCAT()` une múltiples columnas o cadenas de texto en una sola columna de resultado.

SQL

```
-- Seleccionar y mostrar la información de nombre, apellido, teléfono y edad de la tabla estudiante separado por una coma[cite: 1]
SELECT 
    CONCAT(nombre, ', ', apellido, ', ', telefono, ', ', edad) AS informacion_estudiante 
FROM estudiante;[cite: 1]

-- Seleccionar y mostrar la información de nombre, editorial, autor de la tabla libro separado por una coma[cite: 1]
SELECT 
    CONCAT(titulo, ', ', editorial, ', ', autor) AS informacion_libro 
FROM libro;[cite: 1]
```

## 6. Manejo y Búsqueda por Fechas (`YEAR`, `MONTH`, `DAY`)

MySQL incluye funciones nativas para extraer o filtrar partes específicas de una fecha (`YYYY-MM-DD`).

### Filtrar por Año, Mes o Día Específico

SQL

```
-- Obtener estudiantes que nacieron en un año específico (ej. 2005)
SELECT * FROM estudiante 
WHERE YEAR(fecha_nacimiento) = 2005;

-- Obtener estudiantes que nacieron en un mes específico (ej. Mayo -> 5)
SELECT * FROM estudiante 
WHERE MONTH(fecha_nacimiento) = 5;

-- Obtener estudiantes que nacieron un día del mes específico (ej. el día 15)
SELECT * FROM estudiante 
WHERE DAY(fecha_nacimiento) = 15;

-- Combinar año y mes (ej. Nacidos en Mayo de 2004)
SELECT * FROM estudiante 
WHERE YEAR(fecha_nacimiento) = 2004 
  AND MONTH(fecha_nacimiento) = 5;
```

### Filtrar por Fechas Actuales o Rangos Dinámicos

SQL

```
-- Obtener registros de la fecha actual
SELECT * FROM prestamo 
WHERE fecha_prestamo = CURDATE();

-- Obtener préstamos realizados en el año actual
SELECT * FROM prestamo 
WHERE YEAR(fecha_prestamo) = YEAR(CURDATE());
```

### Dar Formato a las Fechas (`DATE_FORMAT`)

Si quieres mostrar la fecha en un formato personalizado (ejemplo: `15/05/2004` o `15 de May de 2004`):

SQL

```
-- Formato DD/MM/YYYY
SELECT 
    nombre, 
    DATE_FORMAT(fecha_nacimiento, '%d/%m/%Y') AS fecha_formateada 
FROM estudiante;

-- Formato con nombre del mes abreviado
SELECT 
    nombre, 
    DATE_FORMAT(fecha_nacimiento, '%d-%b-%Y') AS fecha_texto 
FROM estudiante;
```

## Ver también

- [[MOC-programacion]]
- [[Ruta Mysql]]
- [[SQLmap]]
