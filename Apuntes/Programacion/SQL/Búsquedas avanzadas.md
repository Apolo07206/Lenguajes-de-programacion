---
title: Búsquedas avanzadas
area: programacion
tipo: lenguaje
tags: [programacion, sql, busqueda]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Mysql]]", "[[Rutas]]", "[[Conceptos de mysql]]", "[[WHERE y operadores]]", "[[Consultas básicas con SELECT]]", "[[Uniendo tablas]]", "[[Ordenando y eliminando duplicados]]", "[[SQLmap]]"]
---

# SQL: Operadores avanzados de búsqueda

## Ver también

- [[MOC-programacion]]
- [[Ruta Mysql]]
- [[Rutas]]
- [[Conceptos de mysql]]
- [[WHERE y operadores]]
- [[Consultas básicas con SELECT]]
- [[Uniendo tablas]]
- [[Ordenando y eliminando duplicados]]
- [[SQLmap]]
Además de los operadores de comparación básicos, SQL ofrece operadores que permiten realizar búsquedas más específicas y potentes.

## 1. BETWEEN: búsqueda por rangos

`BETWEEN` permite seleccionar valores dentro de un rango. **Incluye ambos extremos** del rango.

**Sintaxis:**

```sql
SELECT columnas
FROM tabla
WHERE columna BETWEEN valor_minimo AND valor_maximo;
```

**Ejemplo:** buscar videojuegos publicados entre 2010 y 2015.

```sql
SELECT title, year
FROM videogames
WHERE year BETWEEN 2010 AND 2015;
```

Esta consulta devuelve los juegos cuyo año de publicación está entre 2010 y 2015, incluyendo ambos años.

## 2. IN: búsqueda en una lista

`IN` permite comprobar si un valor coincide con alguno de los valores de una lista.

**Sintaxis:**

```sql
SELECT columnas
FROM tabla
WHERE columna IN (valor1, valor2, valor3);
```

**Ejemplo:** buscar videojuegos desarrollados por Nintendo o Valve.

```sql
SELECT title, developer
FROM videogames
WHERE developer IN ('Nintendo', 'Valve');
```

Esta consulta devuelve los videojuegos cuyo desarrollador sea Nintendo o Valve.

**Importante:** los valores de texto deben escribirse entre comillas simples.

## 3. LIKE: búsqueda por patrones

`LIKE` permite buscar textos que coincidan con un patrón determinado. Para ello, utiliza caracteres comodín.

|Comodín|Descripción|Ejemplo|
|---|---|---|
|`%`|Representa cualquier secuencia de caracteres, incluso vacía.|`'The%'`|
|`_`|Representa exactamente un carácter.|`'_alo'`|

### Comodín `%`

Busca textos que coincidan con una secuencia de caracteres.

```sql
-- Títulos que comienzan por "The"
SELECT title
FROM videogames
WHERE title LIKE 'The%';
```

Otros ejemplos:

```sql
-- Títulos que terminan en "of"
WHERE title LIKE '%of';

-- Títulos que contienen "of"
WHERE title LIKE '%of%';
```

### Comodín `_`

Representa exactamente un carácter.

```sql
-- Busca títulos de cuatro caracteres que terminen en "alo"
SELECT title
FROM videogames
WHERE title LIKE '_alo';
```

Por ejemplo, `Halo` coincide con el patrón `_alo`, porque el guion bajo representa la letra `H`.

**Regla para recordar:**

- `%` = cero o más caracteres.
    
- `_` = exactamente un carácter.
    

## 4. Operador NOT: negar búsquedas

`NOT` permite excluir valores o patrones en lugar de seleccionarlos.

### NOT BETWEEN

Selecciona valores que están fuera de un rango.

```sql
SELECT *
FROM videogames
WHERE year NOT BETWEEN 2010 AND 2015;
```

Devuelve los videojuegos publicados antes de 2010 o después de 2015.

### NOT IN

Excluye los valores indicados en una lista.

```sql
SELECT *
FROM videogames
WHERE developer NOT IN ('Nintendo', 'Valve');
```

Devuelve los videojuegos cuyo desarrollador no sea Nintendo ni Valve.

### NOT LIKE

Excluye los textos que coincidan con un patrón.

```sql
SELECT *
FROM videogames
WHERE title NOT LIKE 'The%';
```

Devuelve los videojuegos cuyos títulos no comienzan por `The`.

## 5. Tabla de resumen

|Operador|Función|Ejemplo|
|---|---|---|
|`BETWEEN`|Buscar dentro de un rango, incluidos los extremos.|`year BETWEEN 2010 AND 2015`|
|`IN`|Buscar coincidencias dentro de una lista.|`developer IN ('Nintendo', 'Valve')`|
|`LIKE`|Buscar textos mediante patrones.|`title LIKE 'The%'`|
|`NOT BETWEEN`|Excluir un rango.|`year NOT BETWEEN 2010 AND 2015`|
|`NOT IN`|Excluir valores de una lista.|`developer NOT IN ('Nintendo', 'Valve')`|
|`NOT LIKE`|Excluir textos que coincidan con un patrón.|`title NOT LIKE 'The%'`|

## 6. Tabla de ejemplo: videogames

La tabla utilizada en los ejemplos contiene los siguientes campos:

|Campo|Descripción|
|---|---|
|`id`|Identificador del videojuego.|
|`title`|Título del videojuego.|
|`developer`|Empresa o estudio desarrollador.|
|`year`|Año de publicación.|
|`playtime_hours`|Horas de juego registradas.|

## 7. Diferencias entre SQLite y PostgreSQL

En SQLite, `LIKE` no distingue entre mayúsculas y minúsculas para los caracteres ASCII de forma predeterminada.

Por ejemplo, estas consultas normalmente producen los mismos resultados:

```sql
SELECT *
FROM videogames
WHERE title LIKE 'the%';
```

```sql
SELECT *
FROM videogames
WHERE title LIKE 'The%';
```

En PostgreSQL, `LIKE` sí distingue entre mayúsculas y minúsculas. Si necesitas una búsqueda de texto que no distinga entre ellas, puedes utilizar `ILIKE`:

```sql
SELECT *
FROM videogames
WHERE title ILIKE 'the%';
```

`ILIKE` es una extensión de PostgreSQL y no está disponible de la misma manera en SQLite.

## 8. Ideas clave para memorizar

- `BETWEEN`: buscar por rangos.
    
- `IN`: buscar entre varios valores específicos.
    
- `LIKE`: buscar textos por patrones.
    
- `NOT`: negar o excluir una condición.
    
- `%`: cualquier secuencia de caracteres.
    
- `_`: exactamente un carácter.
    

**Consejo:** practica estos operadores con la tabla `videogames` y modifica los valores de las consultas para comprobar cómo cambian los resultados.