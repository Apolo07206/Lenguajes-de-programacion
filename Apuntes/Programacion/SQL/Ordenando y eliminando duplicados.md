---
title: Ordenando y eliminando duplicados
area: programacion
tipo: lenguaje
tags: [programacion, sql]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Mysql]]", "[[Rutas]]", "[[Conceptos de mysql]]", "[[Búsquedas avanzadas]]", "[[WHERE y operadores]]", "[[Consultas básicas con SELECT]]", "[[Uniendo tablas]]", "[[SQLmap]]"]
---

# SQL: ORDER BY, DISTINCT, LIMIT y OFFSET

Estos comandos y cláusulas permiten controlar cómo se muestran los resultados de una consulta SQL: ordenar filas, eliminar duplicados y limitar la cantidad de resultados.

## 1. ORDER BY: ordenar resultados

`ORDER BY` permite ordenar los resultados de una consulta según una o varias columnas.

**Sintaxis:**

```
SELECT columnas
FROM tabla
ORDER BY columna ASC;
```

Existen dos formas principales de ordenar:

|Dirección|Descripción|
|---|---|
|`ASC`|Orden ascendente, de menor a mayor.|
|`DESC`|Orden descendente, de mayor a menor.|

**Importante:** si no especificas `ASC` o `DESC`, SQL utiliza `ASC` por defecto.

### Ejemplo: ordenar videojuegos por año

```
SELECT title, year
FROM videogames
ORDER BY year ASC;
```

Esta consulta muestra primero los videojuegos más antiguos y después los más recientes.

Para mostrarlos del más reciente al más antiguo:

```
SELECT title, year
FROM videogames
ORDER BY year DESC;
```

### Ordenar por múltiples columnas

Puedes ordenar utilizando varias columnas, separándolas mediante comas.

```
SELECT title, year, playtime_hours
FROM videogames
ORDER BY year DESC, playtime_hours ASC;
```

SQL ordena primero por `year` de forma descendente. Si varios videojuegos tienen el mismo año, los ordena por `playtime_hours` de forma ascendente.

**Regla:** la primera columna establece el orden principal y las siguientes sirven para desempatar.

---

## 2. DISTINCT: eliminar duplicados

`DISTINCT` permite obtener valores únicos, evitando que se repitan en el resultado de una consulta.

**Sintaxis:**

```
SELECT DISTINCT columna
FROM tabla;
```

### Ejemplo: desarrolladores únicos

```
SELECT DISTINCT developer
FROM videogames;
```

Esta consulta devuelve cada desarrollador una sola vez, aunque tenga varios videojuegos registrados.

### DISTINCT con varias columnas

También puedes utilizar `DISTINCT` con más de una columna:

```
SELECT DISTINCT developer, year
FROM videogames;
```

En este caso, se eliminan las filas que tengan la misma combinación de desarrollador y año.

**Importante:** `DISTINCT` no elimina registros de la tabla original. Solo elimina duplicados del resultado de la consulta.

---

## 3. LIMIT: limitar la cantidad de resultados

`LIMIT` establece el número máximo de filas que devuelve una consulta.

**Sintaxis:**

```
SELECT columnas
FROM tabla
LIMIT cantidad;
```

### Ejemplo: mostrar solo 5 videojuegos

```
SELECT *
FROM videogames
LIMIT 5;
```

La consulta devuelve como máximo cinco filas.

### Obtener un Top N

Un uso habitual de `LIMIT` es obtener los primeros resultados de una clasificación.

Por ejemplo, los tres videojuegos con más horas de juego:

```
SELECT title, playtime_hours
FROM videogames
ORDER BY playtime_hours DESC
LIMIT 3;
```

Primero se ordenan los videojuegos de mayor a menor número de horas y después se seleccionan los tres primeros.

El resultado sería:

|title|playtime_hours|
|---|---|
|Minecraft|9999|
|The Witcher 3|100|
|Grand Theft Auto V|80|

**Importante:** `LIMIT` por sí solo no garantiza un orden específico. Si necesitas resultados ordenados, utiliza `ORDER BY`.

---

## 4. OFFSET: saltar filas

`OFFSET` indica cuántas filas se deben saltar antes de comenzar a devolver resultados.

**Sintaxis:**

```
SELECT columnas
FROM tabla
LIMIT cantidad OFFSET salto;
```

- `LIMIT`: cantidad máxima de filas que quieres recibir.
    
- `OFFSET`: cantidad de filas que quieres saltar.
    

### Ejemplo: saltar los primeros 5 videojuegos

```
SELECT *
FROM videogames
LIMIT 5 OFFSET 5;
```

Esta consulta omite las primeras cinco filas y devuelve las siguientes cinco, según el orden de la consulta.

### Paginación

La paginación consiste en dividir los resultados en páginas más pequeñas, algo habitual en páginas web y aplicaciones.

Supongamos que quieres mostrar 10 videojuegos por página:

|Página|LIMIT|OFFSET|
|---|---|---|
|Página 1|10|0|
|Página 2|10|10|
|Página 3|10|20|
|Página 4|10|30|

Para obtener la página 3:

```
SELECT *
FROM videogames
ORDER BY id ASC
LIMIT 10 OFFSET 20;
```

Esta consulta salta los primeros 20 registros y devuelve los siguientes 10.

**Consejo:** utiliza `ORDER BY` para mantener un orden consistente entre páginas.

---

## 5. Combinar ORDER BY, LIMIT y OFFSET

Estos elementos se pueden combinar para construir consultas más específicas.

### Ejemplo: los 3 videojuegos con más horas

```
SELECT title, playtime_hours
FROM videogames
ORDER BY playtime_hours DESC
LIMIT 3;
```

### Ejemplo: los 3 videojuegos más antiguos

```
SELECT title, year
FROM videogames
ORDER BY year ASC
LIMIT 3;
```

### Ejemplo: obtener la segunda página de resultados

```
SELECT title, year
FROM videogames
ORDER BY year DESC, id ASC
LIMIT 5 OFFSET 5;
```

Esta consulta ordena los videojuegos por año descendente, utiliza el identificador para desempatar y devuelve cinco filas después de saltar las primeras cinco.

---

## 6. Tabla de resumen

|Cláusula|Función|Ejemplo|
|---|---|---|
|`ORDER BY`|Ordena los resultados.|`ORDER BY year ASC`|
|`ASC`|Orden ascendente.|`ORDER BY year ASC`|
|`DESC`|Orden descendente.|`ORDER BY year DESC`|
|`DISTINCT`|Elimina duplicados del resultado.|`SELECT DISTINCT developer`|
|`LIMIT`|Limita la cantidad de filas.|`LIMIT 5`|
|`OFFSET`|Salta una cantidad de filas.|`LIMIT 5 OFFSET 10`|

## 7. Ideas clave para memorizar

- `ORDER BY`: decide el orden de los resultados.
    
- `ASC`: de menor a mayor, o alfabéticamente de A a Z.
    
- `DESC`: de mayor a menor, o alfabéticamente de Z a A.
    
- `DISTINCT`: devuelve combinaciones únicas.
    
- `LIMIT`: decide cuántas filas quieres recibir.
    
- `OFFSET`: decide cuántas filas quieres saltar.
    
- Puedes combinar estas cláusulas para crear rankings y paginación.
    

## Ver también

- [[MOC-programacion]]
- [[Ruta Mysql]]
- [[Rutas]]
- [[Conceptos de mysql]]
- [[Búsquedas avanzadas]]
- [[WHERE y operadores]]
- [[Consultas básicas con SELECT]]
- [[Uniendo tablas]]
- [[SQLmap]]
## 8. Ejercicios para practicar

Intenta resolverlos sin mirar las soluciones:

1. Muestra los títulos de todos los videojuegos ordenados alfabéticamente.
    
2. Muestra los videojuegos desde el más reciente hasta el más antiguo.
    
3. Obtén una lista de los desarrolladores sin duplicados.
    
4. Muestra los cinco videojuegos con más horas de juego.
    
5. Obtén los primeros cuatro videojuegos ordenados por año ascendente.
    
6. Muestra los videojuegos ordenados por desarrollador y, dentro de cada desarrollador, por año descendente.
    
7. Obtén cinco videojuegos saltando los primeros cinco, ordenados por `id`.
    
8. Muestra los tres videojuegos más antiguos y presenta primero el año y después el título.
    

**Nota:** estos ejercicios utilizan la tabla `videogames` con las columnas `id`, `title`, `developer`, `year` y `playtime_hours`.