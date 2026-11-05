---
title: Consultas básicas con SELECT
area: programacion
tipo: lenguaje
tags: [programacion, sql]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Mysql]]", "[[Rutas]]", "[[Conceptos de mysql]]", "[[Búsquedas avanzadas]]", "[[WHERE y operadores]]", "[[Uniendo tablas]]", "[[Ordenando y eliminando duplicados]]", "[[SQLmap]]"]
---

Antes de escribir tu primera consulta, vamos a entender dónde viven los datos. Una base de datos es, simplemente, un lugar organizado para almacenar información. Piensa en ella como una hoja de cálculo muy potente: los datos se guardan en **tablas**, y cada tabla tiene **columnas** (los tipos de información) y **filas** (cada registro individual).

Por ejemplo, si quisiéramos guardar información sobre videojuegos, crearíamos una tabla con columnas como `title`, `developer` o `year`. Cada fila sería un videojuego distinto. **SQL** es el lenguaje que usamos para hacer preguntas a esa base de datos: _"¿qué videojuegos hay?"_, _"¿cuáles salieron después de 2010?"_, y mucho más.

## Estructura de una tabla

|**Columnas (tipos de información)**|**title**|**developer**|**year**|
|---|---|---|---|
|**Filas (registros)**|The Legend of Zelda|Nintendo|1986|
|Super Mario Bros.|Nintendo|1985|
|Final Fantasy VII|Square Enix|1997|
|...|...|...|

## La sentencia `SELECT`

La sentencia `SELECT` te permite recuperar datos de una base de datos. Es una de las sentencias más importantes en SQL y la que más usarás en tu día a día.

La sintaxis y estructura básica de una consulta `SELECT` es la siguiente:

SQL

```
SELECT columna1, columna2 FROM tabla;
```

- **`SELECT`**: La palabra clave que indica que queremos recuperar datos.
    
- **`FROM`**: Indica de qué tabla provienen los datos.
    
- **`columnas`**: Los nombres de las columnas que quieres recuperar, separados por comas. Con un asterisco (`*`) recuperas todas.
    
- **`tabla`**: Es el nombre de la tabla de la que deseas obtener los datos.
    

## Tu primera consulta

Para hacer una consulta, primero debemos conocer la estructura de nuestra tabla. En este caso, tenemos una tabla llamada `videogames` con los siguientes campos:

|**id**|**title**|**developer**|**year**|**playtime_hours**|
|---|---|---|---|---|
|1|The Legend of Zelda|Nintendo|1986|20|
|2|Super Mario Bros.|Nintendo|1985|10|
|3|Final Fantasy VII|Square Enix|1997|40|
|4|The Witcher 3|CD Projekt Red|2015|100|
|5|Minecraft|Mojang Studios|2011|9999|
|6|Grand Theft Auto V|Rockstar Games|2013|80|
|7|Dark Souls|FromSoftware|2011|60|
|8|Portal 2|Valve|2011|89|
|9|Red Dead Redemption 2|Rockstar Games|2018|70|
|10|The Last of Us|Naughty Dog|2013|15|
|11|Halo: Combat Evolved|Bungie|2001|10|
|12|God of War|Santa Monica Studio|2018|25|

### 1. Seleccionar todas las columnas

Si quieres recuperar todas las columnas de una tabla, puedes usar el asterisco (`*`):

SQL

```
SELECT * FROM videogames;
```

### 2. Seleccionar columnas específicas

Para recuperar solo algunas columnas, escribe sus nombres separados por comas:

SQL

```
SELECT title, year FROM videogames;
```

> **Ejemplo visual:**
> 
> - `SELECT *` $\rightarrow$ Muestra todas las columnas (`id`, `title`, `year`, `hrs`).
>     
> - `SELECT title, year` $\rightarrow$ Muestra únicamente las columnas elegidas.
>     

## Limitar resultados: `LIMIT`

Cuando una tabla tiene miles o millones de filas, no siempre quieres verlas todas. La cláusula `LIMIT` te permite indicar cuántas filas quieres obtener como máximo:

SQL

```
SELECT * FROM videogames LIMIT 5;
```

Esta consulta devuelve solo las **5 primeras filas** de la tabla. Es muy útil para echar un vistazo rápido a los datos sin cargar toda la tabla.

También puedes combinar `LIMIT` con otras cláusulas. Por ejemplo, para seleccionar solo el título de los 3 primeros videojuegos:

SQL

```
SELECT title FROM videogames LIMIT 3;
```

**Resultado:**

- The Legend of Zelda
    
- Super Mario Bros.
    
- Final Fantasy VII
    
    _(3 filas)_
    

## Comentarios en SQL

Al igual que en otros lenguajes, SQL permite escribir comentarios que el motor de base de datos ignora. Son muy útiles para explicar qué hace una consulta o para desactivar temporalmente una línea.

- Para un comentario de una sola línea, usa dos guiones (`--`):
    
    SQL
    
    ```
    -- Esto es un comentario
    SELECT * FROM videogames; -- Esto también
    ```
    
- Para comentarios de varias líneas, usa `/* ... */`:
    
    SQL
    
    ```
    /* Esta consulta obtiene 
       todos los videojuegos */
    SELECT * FROM videogames;
    ```
    

## Ver también

- [[MOC-programacion]]
- [[Ruta Mysql]]
- [[Rutas]]
- [[Conceptos de mysql]]
- [[Búsquedas avanzadas]]
- [[WHERE y operadores]]
- [[Uniendo tablas]]
- [[Ordenando y eliminando duplicados]]
- [[SQLmap]]
> [!TIP] **Consejo para producción**
> 
> Evita usar `SELECT *` en aplicaciones reales. Seleccionar solo las columnas que necesitas reduce la cantidad de datos que viaja entre la base de datos y tu aplicación, lo que mejora el rendimiento y hace tu código más claro. Reserva `SELECT *` para exploraciones rápidas o pruebas.