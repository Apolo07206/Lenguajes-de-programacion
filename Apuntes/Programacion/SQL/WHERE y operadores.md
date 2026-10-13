---
title: WHERE y operadores
area: programacion
tipo: lenguaje
tags: [programacion, sql, where, filtros]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Mysql]]", "[[Rutas]]", "[[Conceptos de mysql]]", "[[Búsquedas avanzadas]]", "[[Consultas básicas con SELECT]]", "[[Uniendo tablas]]", "[[Ordenando y eliminando duplicados]]", "[[SQLmap]]"]
---

La cláusula `WHERE` te permite filtrar las filas que devuelve una consulta.

> **Ejemplo conceptual:**
> 
> - **Tabla original:** Zelda (1986), Mario (1985), Witcher 3 (2015), Minecraft (2011), FF VII (1997), GTA V (2013)
>     
> - **Condición (`WHERE year < 2000`):** Filtra los juegos anteriores al año 2000.
>     
> - **Resultado:** Zelda (1986) ✓, Mario (1985) ✓, FF VII (1997) ✓
>     

La sintaxis básica es la siguiente:

SQL

```
SELECT columnas 
FROM tabla 
WHERE condición;
```

### Operadores de comparación

Puedes usar los siguientes operadores en tus condiciones:

|**Operador**|**Descripción**|**Ejemplo**|
|---|---|---|
|`=`|Igual a|`year = 2018`|
|`!=`|Distinto de|`year != 2018`|
|`>`|Mayor que|`playtime_hours > 50`|
|`<`|Menor que|`year < 2000`|
|`>=`|Mayor o igual que|`year >= 2010`|
|`<=`|Menor o igual que|`playtime_hours <= 20`|

## Filtrando datos en la práctica

Nuestra tabla `videogames` contiene estos datos:

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
|13|Half-Life|Valve|1998|12|

### 1. Filtrar por valor exacto

Para buscar juegos de un año específico:

SQL

```
SELECT * 
FROM videogames 
WHERE year = 2018;
```

### 2. Filtrar con comparaciones numéricas

Para buscar juegos con muchas horas de juego:

SQL

```
SELECT title, playtime_hours 
FROM videogames 
WHERE playtime_hours > 50;
```

### 3. Filtrar por texto

Para filtrar por texto, debes usar comillas simples alrededor del valor:

SQL

```
SELECT * 
FROM videogames 
WHERE developer = 'Nintendo';
```

## Combinando condiciones: `AND`, `OR` y `NOT`

Hasta ahora hemos filtrado con una sola condición, pero en la práctica necesitarás combinar varias utilizando operadores lógicos.

### `AND`: Ambas condiciones deben cumplirse

`AND` devuelve solo las filas que cumplen todas las condiciones al mismo tiempo:

SQL

```
SELECT * 
FROM videogames 
WHERE year >= 2010 AND playtime_hours > 50;
```

> _Esta consulta devuelve videojuegos publicados a partir de 2010 y que además tengan más de 50 horas de juego. Si un juego cumple solo una de las dos, no aparecerá._

### `OR`: Al menos una condición debe cumplirse

`OR` devuelve las filas que cumplen al menos una de las condiciones planteadas:

SQL

```
SELECT * 
FROM videogames 
WHERE developer = 'Nintendo' OR developer = 'Valve';
```

> _Aquí obtenemos los juegos desarrollados por Nintendo o por Valve. Basta con que una de las dos condiciones sea verdadera._

### `NOT`: Negar una condición

`NOT` invierte una condición, es decir, devuelve únicamente las filas que **no** la cumplen:

SQL

```
SELECT * 
FROM videogames 
WHERE NOT developer = 'Nintendo';
```

> **Explicación visual del cambio (`NOT`):**
> 
> - **Sin `NOT` (`WHERE developer = 'Nintendo'`):** Pasan Zelda (Nintendo) y Mario (Nintendo) $\rightarrow$ _Se quedan 2 filas_.
>     
> - **Con `NOT` (`WHERE NOT developer = 'Nintendo'`):** Se invierte el filtro, descartando a Nintendo y dejando el resto (FF VII, Witcher 3, GTA V, Dark Souls...) $\rightarrow$ _Se quedan 4 filas (ejemplo parcial)_.
>     

## Paréntesis para controlar el orden

Cuando mezclas `AND` y `OR` en la misma consulta, el orden de evaluación importa. SQL evalúa `AND` antes que `OR`, lo que puede dar resultados inesperados si no se especifica.

Usa **paréntesis** para agrupar y dejar claro el orden exacto de ejecución:

SQL

```
SELECT * 
FROM videogames 
WHERE (developer = 'Nintendo' OR developer = 'Valve') 
  AND year > 2000;
```

> [!CAUTION] **¡Cuidado con el orden sin paréntesis!**
> 
> Sin los paréntesis, SQL interpretaría la consulta como: _"juegos de Nintendo, o juegos de Valve publicados después de 2000"_, lo cual cambiaría por completo la lógica. Con los paréntesis, primero se resuelve de forma aislada el grupo `(Nintendo OR Valve)` y luego se cruza estrictamente con la condición del año a ambos.

## Ver también

- [[MOC-programacion]]
- [[Ruta Mysql]]
- [[Rutas]]
- [[Conceptos de mysql]]
- [[Búsquedas avanzadas]]
- [[Consultas básicas con SELECT]]
- [[Uniendo tablas]]
- [[Ordenando y eliminando duplicados]]
- [[SQLmap]]
-  Solo se mostrarán las filas que cumplan la condición especificada.
