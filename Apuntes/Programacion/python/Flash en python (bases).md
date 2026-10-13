---
title: Flash en python (bases)
area: programacion
tipo: proyecto
tags: [programacion, python, flask, mysql]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Rutas]]", "[[Conceptos de mysql]]", "[[Python con Django]]"]
---

# CRUD de Contactos con Flask + MySQL

> App web mínima para **C**rear, **L**eer, **A**ctualizar y **E**liminar contactos (nombre, teléfono, email) guardados en MySQL  

video explicativo    https://youtu.be/IgCfZkR8wME

## 1. Qué hace la app

|Ruta|Método|Acción|CRUD|
|---|---|---|---|
|`/`|GET|Lista todos los contactos|**R**ead|
|`/add`|POST|Inserta un contacto nuevo|**C**reate|
|`/edit/<id>`|GET|Muestra el formulario con los datos del contacto|Read (uno)|
|`/update/<id>`|POST|Guarda los cambios|**U**pdate|
|`/delete/<id>`|GET|Borra el contacto|**D**elete|

## 2. Requisitos

bash

```bash
pip install flask flask-mysqldb
```

- XAMPP/LAMPP con MySQL encendido (`sudo /opt/lampp/lampp start`)
- Base de datos `contacts` con la tabla `contac`:

sql

```sql
CREATE DATABASE contacts;
USE contacts;

CREATE TABLE contac (
    id INT AUTO_INCREMENT PRIMARY KEY,
    fullname VARCHAR(100),
    phone VARCHAR(20),
    email VARCHAR(100)
);
```

## 3. Estructura de carpetas

```
proyecto/
├── app.py
└── templates/
    ├── index.html   # lista + formulario de agregar
    └── edit.html    # formulario de edición
```

> Flask busca los HTML **obligatoriamente** en la carpeta `templates/`.

---

## 4. El código, parte por parte

### 4.1 Imports

python

```python
from flask import Flask, render_template, request, url_for, redirect, flash
from flask_mysqldb import MySQL
```

|Import|Para qué sirve|
|---|---|
|`Flask`|Crea la aplicación|
|`render_template`|Renderiza un HTML de `templates/` y le pasa variables|
|`request`|Accede a los datos que manda el usuario (formularios)|
|`url_for`|Genera la URL a partir del **nombre de la función** (no hardcodeas rutas)|
|`redirect`|Redirige a otra ruta|
|`flash`|Mensajes temporales ("Usuario registrado")|
|`MySQL`|Conexión a MySQL|

### 4.2 Crear la app y la conexión

python

```python
app = Flask(__name__)
mysql = MySQL(app)
```

- `__name__` le dice a Flask dónde está el proyecto (para encontrar `templates/`).
- `MySQL(app)` enlaza la extensión con la app. Aunque se crea **antes** de la config, funciona porque lee los valores `app.config` hasta que se abre la conexión. Igual, es más ordenado poner la config primero.

### 4.3 Configuración

python

```python
app.config['MYSQL_HOST'] = 'localhost'
app.config['MYSQL_USER'] = 'root'
app.config['MYSQL_PASSWORD'] = ''
app.config['MYSQL_DB'] = 'contacts'
app.config['MYSQL_UNIX_SOCKET'] = '/opt/lampp/var/mysql/mysql.sock'

app.secret_key = 'my_secret_key'
```

- `MYSQL_UNIX_SOCKET`: en Linux con XAMPP, MySQL se conecta por un archivo _socket_, no por puerto. Sin esto suele dar error de conexión.
- `secret_key`: **necesaria para `flash()`**, porque los mensajes se guardan en la sesión (cookie firmada). Sin ella da error.

---

### 4.4 READ: listar contactos (`/`)

python

```python
@app.route('/')
def inicio():
    cur = mysql.connection.cursor()
    cur.execute('select * from contac')
    usuarios = cur.fetchall()
    cur.close()
    return render_template('index.html', usuarios=usuarios)
```

Paso a paso:

1. `cursor()` → objeto para ejecutar consultas.
2. `execute(...)` → corre el SQL.
3. `fetchall()` → trae **todas** las filas como tupla de tuplas: `((1, 'Ana', '300...', 'a@x.com'), ...)`
4. `cur.close()` → libera el cursor.
5. `render_template` → manda `usuarios` al HTML.

En `index.html` lo recorres con Jinja:

html

```html
{% for u in usuarios %}
  <tr>
    <td>{{ u[1] }}</td>  <!-- fullname -->
    <td>{{ u[2] }}</td>  <!-- phone -->
    <td>{{ u[3] }}</td>  <!-- email -->
    <td>
      <a href="{{ url_for('edit', id=u[0]) }}">Editar</a>
      <a href="{{ url_for('delete', id=u[0]) }}">Borrar</a>
    </td>
  </tr>
{% endfor %}
```

> `u[0]` es el `id`, `u[1]` fullname, `u[2]` phone, `u[3]` email (el orden de las columnas de la tabla).

---

### 4.5 CREATE: agregar (`/add`)

python

```python
@app.route('/add', methods=['POST'])
def add():
    if request.method == 'POST':
        fullname = request.form['fullname']
        phone = request.form['phone']
        email = request.form['email']

        cur = mysql.connection.cursor()
        cur.execute(
            'INSERT INTO contac (fullname, phone, email) VALUES (%s, %s, %s)',
            (fullname, phone, email)
        )
        mysql.connection.commit()

        flash('Usuario registrado exitosamente')
        cur.close()
        return redirect(url_for('inicio'))
```

Puntos clave:

- `methods=['POST']` → esta ruta **solo** acepta formularios enviados con POST.
- `request.form['fullname']` → el valor del input con `name="fullname"`. **El `name` del HTML debe coincidir exacto.**
- `%s` son **parámetros** y los valores van en una tupla aparte → esto **previene SQL Injection**. Nunca concatenes strings en el SQL.
- `commit()` → sin esto el INSERT **no se guarda** en la BD.
- `redirect` → patrón _Post/Redirect/Get_: evita que al recargar se reenvíe el formulario.

Formulario en HTML:

html

```html
<form action="{{ url_for('add') }}" method="POST">
  <input type="text" name="fullname" placeholder="Nombre completo">
  <input type="text" name="phone" placeholder="Teléfono">
  <input type="email" name="email" placeholder="Correo">
  <button type="submit">Guardar</button>
</form>
```

---

### 4.6 DELETE: borrar (`/delete/<id>`)

python

```python
@app.route('/delete/<string:id>')
def delete(id):
    cur = mysql.connection.cursor()
    cur.execute('delete from contac where id=%s', (id,))
    mysql.connection.commit()
    cur.close()
    return redirect(url_for('inicio'))
```

- `<string:id>` → captura parte de la URL y la pasa como argumento `id`.
- `(id,)` → la **coma** es obligatoria: así Python lo reconoce como tupla de 1 elemento.
- `commit()` otra vez, porque modifica datos.

---

### 4.7 Editar: mostrar el formulario (`/edit/<id>`)

python

```python
@app.route('/edit/<id>')
def edit(id):
    cur = mysql.connection.cursor()
    cur.execute('SELECT * FROM contac WHERE id= %s', (id,))
    edit = cur.fetchall()
    return render_template('edit.html', user=edit[0])
```

- `fetchall()` devuelve una lista de filas; como solo hay una, usas `edit[0]` para sacar **esa** fila.
- Se manda como `user` al HTML.

`edit.html`:

html

```html
<form action="{{ url_for('update', id=user[0]) }}" method="POST">
  <input type="text" name="fullname" value="{{ user[1] }}">
  <input type="text" name="phone" value="{{ user[2] }}">
  <input type="email" name="email" value="{{ user[3] }}">
  <button type="submit">Actualizar</button>
</form>
```

> `value="..."` precarga los datos actuales en los inputs.

---

### 4.8 UPDATE: guardar cambios (`/update/<id>`)

python

```python
@app.route('/update/<id>', methods=['POST'])
def update(id):
    if request.method == 'POST':
        fullname = request.form['fullname']
        phone = request.form['phone']
        email = request.form['email']
        cur = mysql.connection.cursor()
        cur.execute(
            'UPDATE contac set fullname=%s, phone=%s, email=%s WHERE id=%s',
            (fullname, phone, email, id)
        )
        mysql.connection.commit()
        cur.close()
        flash('usuario editado exitosamente')
        return redirect(url_for('inicio'))
```

- Ojo con el **orden de la tupla**: debe coincidir con el orden de los `%s`.
- **No olvides el `WHERE id=%s`**: sin él actualizas **todos** los registros.

---

### 4.9 Arrancar la app

python

```python
if __name__ == '__main__':
    app.run(debug=True)
```

- `__name__ == '__main__'` → solo corre si ejecutas el archivo directo (`python app.py`), no si lo importas.
- `debug=True` → recarga automática al guardar y muestra errores detallados. **Solo para desarrollo.**

---

## 5. Mostrar los `flash` en el HTML

Poner en `index.html` (arriba):

html

```html
{% with messages = get_flashed_messages() %}
  {% if messages %}
    {% for m in messages %}
      <div class="alert">{{ m }}</div>
    {% endfor %}
  {% endif %}
{% endwith %}
```

## 6. Flujo completo (resumen mental)

```
Navegador → Ruta Flask → Cursor → SQL (con %s) → commit (si modifica) → redirect/render → HTML
```

Patrón que se repite en **todas** las rutas:

1. Leer datos (`request.form` o parámetro de la URL)
2. Abrir cursor
3. `execute` con parámetros
4. `commit` (solo INSERT/UPDATE/DELETE)
5. Cerrar cursor
6. `redirect` o `render_template`

## 7. Mejoras pendientes (para pulir el proyecto)

- [ ]  **Delete por GET es mala práctica**: cualquier link o bot puede borrar datos. Lo correcto es POST (con un mini formulario/botón).
- [ ]  En `edit()` falta `cur.close()`.
- [ ]  El `if request.method == 'POST'` es redundante, porque la ruta ya solo acepta POST.
- [ ]  Credenciales y `secret_key` hardcodeadas → pasarlas a **variables de entorno** (`.env` + `python-dotenv`).
- [ ]  Contraseña vacía de `root` → crear un usuario propio con contraseña.
- [ ]  Validar datos (campos vacíos, formato de email) antes del INSERT.
- [ ]  Manejar errores con `try/except`.
- [ ]  Quitar `debug=True` en producción.
- [ ]  Agregar login y control de accesos.
- [ ]  Usar `DictCursor` para acceder por nombre (`u['fullname']`) en vez de índices (`u[1]`).

## 8. Errores comunes

|Error|Causa probable|
|---|---|
|`Can't connect to MySQL server`|MySQL apagado o socket incorrecto|
|`RuntimeError: session is unavailable`|Falta `secret_key`|
|`BadRequestKeyError`|El `name` del input no coincide con `request.form['...']`|
|Los datos no se guardan|Falta `mysql.connection.commit()`|
|`TemplateNotFound`|El HTML no está en `templates/`|

## Ver también

- [[MOC-programacion]]
- [[Ruta Python]]
- [[Rutas]]
- [[Conceptos de mysql]]
- [[Python con Django]]
