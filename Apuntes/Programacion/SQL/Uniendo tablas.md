---
title: Uniendo tablas
area: programacion
tipo: lenguaje
tags: [programacion, sql, join]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Mysql]]", "[[Rutas]]", "[[Conceptos de mysql]]", "[[Búsquedas avanzadas]]", "[[WHERE y operadores]]", "[[Consultas básicas con SELECT]]", "[[Ordenando y eliminando duplicados]]", "[[SQLmap]]"]
---

# SQL: JOIN

> [!info] Idea en una línea
> Los datos están repartidos en varias tablas. `JOIN` las pega para verlos juntos.

## Las tablas del ejemplo

**developers**

| id | name | country | founded_year |
|---|---|---|---|
| 1 | Nintendo | Japón | 1889 |
| 2 | Square Enix | Japón | 1975 |
| 3 | CD Projekt Red | Polonia | 1994 |
| 4 | Mojang Studios | Suecia | 2009 |
| 5 | Rockstar Games | Estados Unidos | 1998 |
| 6 | FromSoftware | Japón | 1986 |
| 7 | Valve | Estados Unidos | 1996 |
| 8 | Naughty Dog | Estados Unidos | 1984 |
| 9 | Bungie | Estados Unidos | 1991 |
| 10 | Santa Monica Studio | Estados Unidos | 1999 |

**videogames**

| id | title | developer_id | year | playtime_hours |
|---|---|---|---|---|
| 1 | The Legend of Zelda | 1 | 1986 | 20 |
| 2 | Super Mario Bros. | 1 | 1985 | 10 |
| 3 | Final Fantasy VII | 2 | 1997 | 40 |
| 4 | The Witcher 3 | 3 | 2015 | 100 |
| 5 | Minecraft | 4 | 2011 | 9999 |
| 6 | Grand Theft Auto V | 5 | 2013 | 80 |
| 7 | Dark Souls | 6 | 2011 | 60 |
| 8 | Portal 2 | 7 | 2011 | 8 |
| 9 | Red Dead Redemption 2 | 5 | 2018 | 70 |
| 10 | The Last of Us | 8 | 2013 | 15 |
| 11 | Halo: Combat Evolved | 9 | 2001 | 10 |
| 12 | God of War | 10 | 2018 | 25 |

> [!tip] Cómo se conectan
> `videogames.developer_id` (clave foránea) apunta a `developers.id` (clave primaria).
> Zelda tiene `developer_id = 1` → ese 1 es Nintendo.

---

## 1. INNER JOIN

Une cada fila con **la que le corresponde**. Solo salen las filas que tienen coincidencia.

### Problema
Quiero el título de cada juego y el nombre de su empresa.

### Cómo lo pienso
1. ¿Qué datos necesito y de qué tabla? `title` → videogames, `name` → developers.
2. ¿Qué columna conecta las tablas? `developer_id = id`.
3. Escribo la consulta.

```sql
SELECT videogames.title, developers.name
FROM videogames
INNER JOIN developers
    ON videogames.developer_id = developers.id;
```

| title | name |
|---|---|
| The Legend of Zelda | Nintendo |
| Super Mario Bros. | Nintendo |
| Final Fantasy VII | Square Enix |
| ... | ... |

### Plantilla
```sql
SELECT tabla1.columna, tabla2.columna
FROM tabla1
INNER JOIN tabla2
    ON tabla1.clave_foranea = tabla2.id;
```

> [!note] Cómo lo hace SQL por dentro
> Zelda (`developer_id = 1`) → busca `id = 1` en developers → Nintendo.
> Repite lo mismo con cada fila.

---

## 2. Alias

Apodos para las tablas. No cambia el resultado, solo escribes menos.

```sql
SELECT v.title, d.name
FROM videogames AS v
INNER JOIN developers AS d
    ON v.developer_id = d.id;
```

- `v` = videogames, `d` = developers.
- `AS` es opcional: `FROM videogames v` también funciona.

> [!warning] Error común
> El alias que declaras en el `FROM`/`JOIN` es el que debes usar en el `SELECT` y el `ON`.
> Si declaras `v` y escribes `game1.title`, da error.

También puedes renombrar **columnas** con `AS`:
```sql
SELECT v.title AS juego, d.name AS empresa
```

---

## 3. WHERE con JOIN

Primero unes, luego filtras con columnas de **cualquiera** de las dos tablas.

### Problema
Solo los juegos hechos por empresas de Japón.

```sql
SELECT v.title, d.country
FROM videogames AS v
INNER JOIN developers AS d ON v.developer_id = d.id
WHERE d.country = 'Japón';
```

| title | country |
|---|---|
| The Legend of Zelda | Japón |
| Super Mario Bros. | Japón |
| Final Fantasy VII | Japón |
| Dark Souls | Japón |

> [!warning] Dónde va cada cosa
> `ON` = cómo se conectan las tablas.
> `WHERE` = el filtro.
> No metas el filtro dentro del `ON`.

---

## 4. SELF JOIN

Es unir una tabla **consigo misma**. Usas la misma tabla dos veces, con dos alias distintos, como si fueran dos copias.

> [!tip] Cuándo lo necesito
> Cuando una fila apunta a **otra fila de la misma tabla**, o cuando quiero comparar filas entre sí.

### Caso 1: empleados y jefes (el caso real)

Un jefe también es un empleado, así que no hay otra tabla.

| id | nombre | jefe_id |
|---|---|---|
| 1 | Ana | NULL |
| 2 | Luis | 1 |
| 3 | Marta | 1 |
| 4 | Pedro | 2 |
| 5 | Sofía | 2 |

```sql
SELECT e.nombre AS empleado, j.nombre AS jefe
FROM empleados AS e
INNER JOIN empleados AS j ON e.jefe_id = j.id;
```

| empleado | jefe |
|---|---|
| Luis | Ana |
| Marta | Ana |
| Pedro | Luis |
| Sofía | Luis |

Paso a paso con Pedro:
```
Pedro (en e): jefe_id = 2 → busco id = 2 en j → Luis → Pedro | Luis
```
Es **igual** que el INNER JOIN normal (Zelda → Nintendo). La única diferencia: buscas en la misma tabla en vez de otra.

Ana no sale porque su `jefe_id` es NULL (no tiene jefe).

### Caso 2: pares de filas (el del documento)

Pares de juegos del mismo desarrollador:

```sql
SELECT v1.title AS juego_1,
       v2.title AS juego_2,
       v1.developer_id
FROM videogames AS v1
INNER JOIN videogames AS v2
    ON v1.developer_id = v2.developer_id
    AND v1.id < v2.id;
```

| juego_1 | juego_2 | developer_id |
|---|---|---|
| The Legend of Zelda | Super Mario Bros. | 1 |
| Grand Theft Auto V | Red Dead Redemption 2 | 5 |

> [!note] ¿Para qué sirve `v1.id < v2.id`?
> Sin esa condición, un juego se empareja **consigo mismo** y cada par sale **dos veces** (Zelda-Mario y Mario-Zelda).
> Con `<` solo queda un par por combinación.
> Solo hace falta cuando comparas filas del mismo tipo. En el caso jefe → empleado no se necesita.

> [!tip] Para "todos los juegos de cada empresa" NO uses SELF JOIN
> Eso es un INNER JOIN normal con `ORDER BY`. El SELF JOIN de pares es más didáctico que práctico.

---

## 5. CROSS JOIN

**Todos con todos.** Sin `ON`.

### Problema
Tengo 4 productos y 3 tallas. Quiero todas las combinaciones.

```sql
SELECT p.nombre, t.tamano
FROM productos AS p
CROSS JOIN tamanos AS t;
```

```
Camiseta → S, M, L
Pantalón → S, M, L
Gorra    → S, M, L
Chaqueta → S, M, L
```

4 productos × 3 tallas = **12 filas**.

Con los videojuegos: 12 juegos × 10 empresas = **120 filas** (salen parejas sin sentido como Zelda-Valve, y está bien).

Sirve para: producto × talla × color, calendarios (meses × años), datos de prueba.

> [!warning] Cuidado
> Con tablas grandes genera muchísimas filas.

---

## Resumen

| Tipo | En una frase | ¿Lleva `ON`? |
|---|---|---|
| **INNER JOIN** | Pega cada fila con la que le corresponde | Sí |
| **SELF JOIN** | La misma tabla dos veces (alias distintos) | Sí |
| **CROSS JOIN** | Todos con todos | No |

```sql
-- INNER JOIN
FROM a INNER JOIN b ON a.x = b.y

-- SELF JOIN
FROM t AS t1 INNER JOIN t AS t2 ON t1.x = t2.x AND t1.id < t2.id

-- CROSS JOIN
FROM a CROSS JOIN b
```

---

## Errores que me pasaron

> [!bug] Revisar siempre
> 1. **Alias que no coinciden:** declaro `juego1` y uso `game1` → error.
> 2. **`and` colgado en el `ON`:** `ON a.x = b.y and ;` → error.
> 3. **Filtro en el `ON`:** va en el `WHERE`.
> 4. **En plataformas de ejercicios:** leer qué columnas pide y cómo las llama (`AS juego_1`). Si la lógica está bien pero el formato de salida no coincide, marca error.

---

## Ver también

- [[MOC-programacion]]
- [[Ruta Mysql]]
- [[Rutas]]
- [[Conceptos de mysql]]
- [[Búsquedas avanzadas]]
- [[WHERE y operadores]]
- [[Consultas básicas con SELECT]]
- [[Ordenando y eliminando duplicados]]
- [[SQLmap]]
## Ejercicios resueltos

```sql
-- 1. Título + desarrollador
SELECT v.title, d.name
FROM videogames v
INNER JOIN developers d ON v.developer_id = d.id;

-- 2. Título, año y país del desarrollador
SELECT v.title, v.year, d.country
FROM videogames v
INNER JOIN developers d ON v.developer_id = d.id;

-- 3. Solo juegos de Japón
SELECT v.title
FROM videogames v
INNER JOIN developers d ON v.developer_id = d.id
WHERE d.country = 'Japón';

-- 4. Título + año de fundación del desarrollador
SELECT v.title, d.founded_year
FROM videogames v
INNER JOIN developers d ON v.developer_id = d.id;

-- 5. Con alias (igual que el 1)

-- 6. Pares del mismo desarrollador
SELECT v1.title AS juego_1, v2.title AS juego_2, v1.developer_id
FROM videogames v1
INNER JOIN videogames v2
    ON v1.developer_id = v2.developer_id AND v1.id < v2.id;

-- 7. CROSS JOIN (da 120 filas)
SELECT v.title, d.name
FROM videogames v
CROSS JOIN developers d;
```

## Siguiente tema
- [ ] LEFT JOIN (trae todo de la tabla izquierda, aunque no tenga coincidencia)