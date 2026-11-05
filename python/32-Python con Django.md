---
aliases: ["Python con Django"]
title: Python con Django
area: programacion
tipo: lenguaje
tags: [programacion, python, django, web]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Rutas]]", "[[01-Conceptos de mysql|Conceptos de mysql]]", "[[31-Flash en python (bases)|Flash en python (bases)]]", "[[30-Sockets|Sockets]]"]
---
# Python con Django

## 1. ¿Qué es Django?

Django es un framework de Python que permite desarrollar aplicaciones web de forma rápida, segura y organizada.

Entre sus funcionalidades están:

- Manejo de URLs y peticiones HTTP.
- Conexión con bases de datos (ORM).
- Gestión de usuarios, autenticación y permisos.
- Formularios.
- Generación de páginas HTML con plantillas (templates).
- Panel de administración automático.
- Creación de APIs y endpoints.
- Administración de datos mediante modelos y migraciones.

## 2. Crear la carpeta del proyecto

```bash
mkdir django_aprendizaje
cd django_aprendizaje
```

- `mkdir`: crea una carpeta.
- `cd`: entra en una carpeta.

## 3. Crear el entorno virtual

```bash
python3 -m venv env
```

Crea un entorno virtual llamado `env` para instalar las dependencias del proyecto sin mezclarlas con las de otros proyectos.

### Activar el entorno virtual

```bash
source env/bin/activate        # Linux / Mac
env\Scripts\activate           # Windows
```

Al activarlo, aparecerá `(env)` al inicio de la línea de comandos.

## 4. Instalar Django

```bash
pip install django
```

### Comprobar la instalación

```bash
django-admin --version
```

## 5. Crear el proyecto

```bash
django-admin startproject tienda .
```

- `django-admin`: herramienta para administrar Django.
- `startproject`: crea la estructura inicial de un proyecto.
- `tienda`: nombre del proyecto.
- `.`: indica que los archivos se creen en la carpeta actual.

### Estructura inicial

```
django_aprendizaje/
├── env/
├── manage.py
└── tienda/
    ├── __init__.py
    ├── settings.py
    ├── urls.py
    ├── asgi.py
    └── wsgi.py
```

### Archivos principales

- `manage.py`: permite ejecutar comandos de administración.
- `settings.py`: contiene la configuración del proyecto.
- `urls.py`: define las rutas principales.
- `asgi.py`: punto de entrada para servidores compatibles con ASGI.
- `wsgi.py`: punto de entrada para servidores compatibles con WSGI.
- `__init__.py`: permite tratar la carpeta como un paquete de Python.

## 6. Ejecutar el servidor de desarrollo

```bash
python manage.py runserver
```

Abre en el navegador:

```
http://127.0.0.1:8000/
```

Si todo funciona, aparecerá la página de bienvenida de Django. Para detener el servidor: `Ctrl + C`.

## 7. Crear una aplicación (app)

Un proyecto Django puede contener varias aplicaciones. Cada app se encarga de una funcionalidad específica (`productos`, `usuarios`, `pedidos`).

```bash
python manage.py startapp productos
```

### Estructura de la aplicación

```
productos/
├── migrations/
│   └── __init__.py
├── __init__.py
├── admin.py
├── apps.py
├── models.py
├── tests.py
└── views.py
```

### Archivos principales de una app

- `admin.py`: configura cómo se muestran los modelos en el panel administrativo.
- `apps.py`: configuración de la aplicación.
- `models.py`: define los modelos que representan los datos.
- `tests.py`: pruebas automatizadas.
- `views.py`: vistas que procesan las peticiones.
- `migrations/`: archivos de migración de la base de datos.

> [!warning] Importante Django **no** crea `urls.py` dentro de cada app. Lo creas tú manualmente.

## 8. Registrar la aplicación en el proyecto

Abre `tienda/settings.py`, busca `INSTALLED_APPS` y añade la app:

```python
INSTALLED_APPS = [
    # Aplicaciones incluidas con Django
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    # Aplicaciones propias
    'productos.apps.ProductosConfig',   # o simplemente 'productos'
	 'categoria' # tambien se puede de esta manera
]
```

`productos.apps.ProductosConfig` apunta a la clase definida en `productos/apps.py`:

```python
from django.apps import AppConfig


class ProductosConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'productos'
```

Registrar la app permite que Django reconozca sus modelos, templates y demás.

---

## 9. Modelos en Django

Un modelo = una clase de Python que representa **una tabla** de la base de datos. Cada atributo = una columna.

- Se definen en `productos/models.py`.
- Heredan de `models.Model`.
- Django crea automáticamente el campo `id` (llave primaria).

### 9.1. Modelo básico

```python
from django.db import models


class Producto(models.Model):
    nombre = models.CharField(max_length=100)
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.PositiveIntegerField(default=0)
    creado_en = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.nombre
```

### 9.2. Tipos de campos más usados

|Campo|Para qué sirve|
|---|---|
|`CharField(max_length=100)`|Texto corto (obliga `max_length`).|
|`TextField()`|Texto largo (descripciones).|
|`IntegerField()`|Entero.|
|`PositiveIntegerField()`|Entero ≥ 0.|
|`DecimalField(max_digits, decimal_places)`|Decimales exactos (precios).|
|`FloatField()`|Decimal aproximado.|
|`BooleanField(default=True)`|Verdadero / falso.|
|`DateField()`|Solo fecha.|
|`DateTimeField()`|Fecha y hora.|
|`EmailField()`|Texto validado como correo.|
|`URLField()`|Texto validado como URL.|
|`ImageField(upload_to='fotos/')`|Imagen (requiere `pip install pillow`).|
|`SlugField()`|Texto para URLs (`mi-producto`).|

### 9.3. Opciones comunes de los campos

|Opción|Qué hace|
|---|---|
|`null=True`|Permite `NULL` en la base de datos.|
|`blank=True`|Permite dejarlo vacío en formularios/admin.|
|`default=valor`|Valor por defecto.|
|`unique=True`|No se puede repetir.|
|`choices=[...]`|Limita a una lista de opciones.|
|`verbose_name='...'`|Nombre legible en el admin.|
|`auto_now_add=True`|Fecha al **crear** el registro.|
|`auto_now=True`|Fecha en cada **actualización**.|

> [!tip] null vs blank `null` es de la **base de datos**, `blank` es de la **validación**. Para campos de texto normalmente se usa solo `blank=True`. Para fechas/números opcionales: `null=True, blank=True`.

### 9.4. Choices (lista de opciones)

```python
class Producto(models.Model):
    ESTADOS = [
        ('act', 'Activo'),
        ('ina', 'Inactivo'),
        ('agt', 'Agotado'),
    ]
    nombre = models.CharField(max_length=100)
    estado = models.CharField(max_length=3, choices=ESTADOS, default='act')
```

Se guarda `'act'` en la base de datos; en el admin se ve `Activo`. Para mostrarlo legible en un template: `{{ producto.get_estado_display }}`.

### 9.5. Relaciones entre modelos

**ForeignKey (uno a muchos):** una categoría tiene muchos productos.

```python
from django.db import models
from django.contrib.auth.models import User


class Categoria(models.Model):
    nombre = models.CharField(max_length=50)

    def __str__(self):
        return self.nombre


class Producto(models.Model):
    nombre = models.CharField(max_length=100)
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.PositiveIntegerField(default=0)
    categoria = models.ForeignKey(
        Categoria,
        on_delete=models.CASCADE,
        related_name='productos',
    )
    dueno = models.ForeignKey(User, on_delete=models.CASCADE, null=True, blank=True)
    creado_en = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.nombre
```

- `on_delete=models.CASCADE`: si borras la categoría, se borran sus productos.
- `on_delete=models.PROTECT`: impide borrar la categoría si tiene productos.
- `on_delete=models.SET_NULL` (con `null=True`): pone `NULL` al borrar.
- `related_name='productos'`: permite hacer `categoria.productos.all()`.

**Otras relaciones:**

|Relación|Campo|Ejemplo|
|---|---|---|
|Uno a muchos|`ForeignKey`|Categoría → Productos|
|Muchos a muchos|`ManyToManyField`|Productos ↔ Etiquetas|
|Uno a uno|`OneToOneField`|Usuario ↔ Perfil|

### 9.6. Clase Meta

Configuración extra del modelo:

```python
class Producto(models.Model):
    # ... campos ...

    class Meta:
        ordering = ['-creado_en']              # orden por defecto (más nuevo primero)
        verbose_name = 'Producto'
        verbose_name_plural = 'Productos'
```

### 9.7. Métodos del modelo

```python
class Producto(models.Model):
    # ... campos ...

    def __str__(self):
        return self.nombre

    def hay_stock(self):
        return self.stock > 0
```

En un template: `{% if producto.hay_stock %}` (sin paréntesis).

## 10. Crear y aplicar migraciones

Cada vez que creas o cambias un modelo hay que registrar el cambio y aplicarlo a la base de datos.

```bash
python manage.py makemigrations
python manage.py migrate
```

|Comando|Función|
|---|---|
|`makemigrations`|Genera los archivos que describen los cambios de los modelos.|
|`migrate`|Aplica esos cambios a la base de datos.|
|`showmigrations`|Muestra las migraciones y su estado.|
|`sqlmigrate productos 0001`|Muestra el SQL de una migración.|

- `makemigrations` solo **crea el archivo** (`0001_initial.py`), no toca la base de datos.
- `migrate` es el que **aplica** los cambios.
- Por defecto Django usa SQLite.
- Si solo cambias una vista, una ruta o un template, **no** necesitas migraciones.

## 11. Panel de administración (admin) — lo básico

Django trae un panel para ver y editar los datos sin programar nada extra.

### 11.1. Crear el superusuario

```bash
python manage.py createsuperuser
```

Te pide usuario, correo (opcional) y contraseña.

### 11.2. Entrar al panel

Con el servidor corriendo:

```
http://127.0.0.1:8000/admin/
```

### 11.3. Registrar modelos

Los modelos **no aparecen solos**. Hay que registrarlos en `productos/admin.py`.

**Forma simple:**

```python
from django.contrib import admin
from .models import Producto, Categoria

admin.site.register(Producto)
admin.site.register(Categoria)
```

**Forma personalizada (recomendada):**

```python
from django.contrib import admin
from .models import Producto, Categoria


@admin.register(Producto)
class ProductoAdmin(admin.ModelAdmin):
    list_display = ('nombre', 'precio', 'stock', 'categoria')   # columnas de la lista
    search_fields = ('nombre',)                                  # barra de búsqueda
    list_filter = ('categoria', 'creado_en')                     # filtros laterales
    list_editable = ('stock',)                                   # editar directo en la lista
    ordering = ('nombre',)


@admin.register(Categoria)
class CategoriaAdmin(admin.ModelAdmin):
    list_display = ('nombre',)
```

|Opción|Qué hace|
|---|---|
|`list_display`|Columnas que se ven en la lista.|
|`search_fields`|Campos donde busca la barra de búsqueda.|
|`list_filter`|Filtros del lado derecho.|
|`list_editable`|Campos editables sin entrar al registro.|
|`ordering`|Orden de la lista.|
|`readonly_fields`|Campos que se ven pero no se editan.|

> [!tip] El admin también sirve para crear **usuarios, grupos y permisos** (se usa en la sección 19).

## 12. El ORM: consultar y modificar la base de datos

El ORM permite usar la base de datos con Python en vez de SQL. Se accede mediante `Modelo.objects`.

### 12.1. Probar en la shell

```bash
python manage.py shell
```

```python
from productos.models import Producto, Categoria
```

### 12.2. Crear registros

```python
# Forma 1: create (crea y guarda de una)
Producto.objects.create(nombre="Teclado", precio=150000, stock=10, categoria=cat)

# Forma 2: instanciar + save
p = Producto(nombre="Ratón", precio=60000, stock=5, categoria=cat)
p.save()
```

### 12.3. Leer registros

```python
Producto.objects.all()                    # todos
Producto.objects.get(id=1)                # UNO solo (si no existe: DoesNotExist)
Producto.objects.filter(stock=0)          # varios que cumplan la condición
Producto.objects.exclude(stock=0)         # todos menos los que cumplan
Producto.objects.first()                  # el primero
Producto.objects.last()                   # el último
Producto.objects.count()                  # cuántos hay
Producto.objects.exists()                 # True / False
Producto.objects.order_by('precio')       # ascendente
Producto.objects.order_by('-precio')      # descendente (con el signo -)
Producto.objects.all()[:5]                # los primeros 5 (LIMIT)
```

> [!warning] get() vs filter()
> 
> - `get()` devuelve **un objeto** y lanza error si no hay ninguno (`DoesNotExist`) o si hay más de uno (`MultipleObjectsReturned`).
> - `filter()` devuelve un **QuerySet** (lista de objetos), aunque esté vacío.

### 12.4. Filtros con `__` (lookups)

```python
Producto.objects.filter(precio__gt=100000)          # mayor que
Producto.objects.filter(precio__gte=100000)         # mayor o igual
Producto.objects.filter(precio__lt=50000)           # menor que
Producto.objects.filter(precio__lte=50000)          # menor o igual
Producto.objects.filter(nombre__contains="Tec")     # contiene (distingue mayúsculas)
Producto.objects.filter(nombre__icontains="tec")    # contiene (NO distingue mayúsculas)
Producto.objects.filter(nombre__startswith="T")     # empieza por
Producto.objects.filter(id__in=[1, 2, 3])           # está en la lista
Producto.objects.filter(descripcion__isnull=True)   # es NULL
```

Se pueden combinar (es un **AND**):

```python
Producto.objects.filter(stock__gt=0, precio__lt=100000)
```

Se pueden encadenar:

```python
Producto.objects.filter(stock__gt=0).order_by('-precio')[:3]
```

### 12.5. OR con Q

```python
from django.db.models import Q

Producto.objects.filter(Q(nombre__icontains="tec") | Q(precio__lt=50000))   # OR
Producto.objects.filter(Q(stock__gt=0) & ~Q(categoria__nombre="Ofertas"))   # AND + NOT
```

### 12.6. Actualizar

```python
# Un registro
p = Producto.objects.get(id=1)
p.stock = 20
p.save()

# Varios a la vez
Producto.objects.filter(stock=0).update(estado='agt')
```

### 12.7. Eliminar

```python
p = Producto.objects.get(id=1)
p.delete()

Producto.objects.filter(stock=0).delete()    # varios
```

### 12.8. Relaciones

```python
p = Producto.objects.get(id=1)
p.categoria.nombre                     # de producto → categoría

cat = Categoria.objects.get(nombre="Teclados")
cat.productos.all()                    # de categoría → sus productos (por related_name)

Producto.objects.filter(categoria__nombre="Teclados")   # filtrar por campo de la relación
```

### 12.9. Agregaciones

```python
from django.db.models import Sum, Avg, Max, Min, Count

Producto.objects.aggregate(Sum('stock'))        # {'stock__sum': 150}
Producto.objects.aggregate(Avg('precio'))       # {'precio__avg': ...}
Producto.objects.aggregate(total=Count('id'))   # {'total': 12}
```

### 12.10. Resumen rápido

|Quiero...|Código|
|---|---|
|Todos|`Modelo.objects.all()`|
|Uno|`Modelo.objects.get(id=1)`|
|Filtrar|`Modelo.objects.filter(campo=valor)`|
|Excluir|`Modelo.objects.exclude(campo=valor)`|
|Ordenar|`Modelo.objects.order_by('-campo')`|
|Contar|`Modelo.objects.count()`|
|Crear|`Modelo.objects.create(...)`|
|Editar|`obj.campo = x` → `obj.save()`|
|Borrar|`obj.delete()`|

## 13. Vistas

Las vistas reciben las peticiones HTTP y deciden qué respuesta devolver.

### 13.1. Vistas que devuelven texto o JSON

```python
from django.http import HttpResponse, JsonResponse


def saludo(request):
    return HttpResponse("Hola, Django!")


def lista_productos_json(request):
    productos = list(Producto.objects.values('id', 'nombre', 'precio'))
    return JsonResponse(productos, safe=False)
```

- `request`: información de la petición recibida.
- `HttpResponse`: respuesta de texto.
- `JsonResponse`: respuesta en JSON (`safe=False` para devolver una lista).
- `.values(...)`: convierte el QuerySet en diccionarios (así se puede pasar a JSON).

### 13.2. Vistas que devuelven HTML (con datos de la base de datos)

```python
from django.shortcuts import render, get_object_or_404
from .models import Producto


def lista_productos(request):
    productos = Producto.objects.all()
    contexto = {'productos': productos}
    return render(request, 'productos/lista.html', contexto)


def detalle_producto(request, pk):
    producto = get_object_or_404(Producto, pk=pk)
    return render(request, 'productos/detalle.html', {'producto': producto})
```

`render(request, plantilla, contexto)`:

- `request`: la petición.
- `'productos/lista.html'`: ruta de la plantilla.
- `contexto`: diccionario con los datos que se envían al HTML. La **clave** es el nombre con el que lo usas en el template.

## 14. Rutas de la aplicación

Crea manualmente `productos/urls.py`:

```python
from django.urls import path
from . import views


urlpatterns = [
    path('', views.lista_productos, name='lista_productos'),
    path('<int:pk>/', views.detalle_producto, name='detalle_producto'),
    path('saludo/', views.saludo, name='saludo'),
]
```

- `path()`: define una ruta y la vista que la atiende.
- `''`: ruta raíz de la app.
- `<int:pk>`: **parámetro dinámico**, captura un entero y lo envía a la vista como `pk`.
- `name`: nombre de la ruta, sirve para usarla en templates con `{% url %}`.

Otros convertidores: `<str:nombre>`, `<slug:slug>`, `<int:id>`.

No se escribe el prefijo `productos/` aquí; se añade desde el proyecto principal.

## 15. Conectar las rutas con el proyecto principal

`tienda/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include


urlpatterns = [
    path('admin/', admin.site.urls),
    path('productos/', include('productos.urls')),
]
```

`include()` delega las rutas que empiezan por `productos/` al archivo `productos/urls.py`.

|Proyecto principal|Ruta de la app|URL final|
|---|---|---|
|`productos/`|`''`|`/productos/`|
|`productos/`|`saludo/`|`/productos/saludo/`|
|`productos/`|`<int:pk>/`|`/productos/5/`|

## 16. Templates (HTML con Django)

> [!note] ¿Jinja o Django? Django trae su propio sistema de plantillas: **DTL (Django Template Language)**. Se parece muchísimo a Jinja2 (la sintaxis de `{{ }}` y `{% %}` es casi igual), pero **no es lo mismo**. Es el que viene por defecto. Diferencias que más importan: en DTL no se pueden llamar métodos con argumentos, y se escribe `{% endif %}` / `{% endfor %}` (igual que Jinja) pero los filtros usan `|filtro:argumento` en vez de `|filtro(argumento)`.

### 16.1. Dónde van los templates

Opción A (por app):

```
productos/
└── templates/
    └── productos/
        ├── lista.html
        └── detalle.html
```

La carpeta repetida (`templates/productos/`) evita choques de nombres entre apps. Funciona sin configurar nada extra porque `APP_DIRS` ya es `True`.

Opción B (carpeta global) — en `settings.py`:

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],      # <- añadir esto
        'APP_DIRS': True,
        # ...
    },
]
```

### 16.2. Sintaxis básica

|Sintaxis|Para qué|
|---|---|
|`{{ variable }}`|Mostrar un valor.|
|`{% etiqueta %}`|Lógica (bucles, condiciones, etc.).|
|`{# comentario #}`|Comentario.|
|`{{ valor\|filtro }}`|Aplicar un filtro al valor.|

### 16.3. Mostrar datos

```html
<h1>{{ producto.nombre }}</h1>
<p>Precio: {{ producto.precio }}</p>
<p>Categoría: {{ producto.categoria.nombre }}</p>
```

Con el punto se accede a atributos, relaciones, métodos (sin paréntesis) y claves de diccionarios.

### 16.4. Bucles: `for`

Vista:

```python
def lista_productos(request):
    productos = Producto.objects.all()
    return render(request, 'productos/lista.html', {'productos': productos})
```

Template `lista.html`:

```html
<h1>Productos</h1>

<ul>
  {% for p in productos %}
    <li>{{ forloop.counter }}. {{ p.nombre }} - ${{ p.precio }}</li>
  {% empty %}
    <li>No hay productos todavía.</li>
  {% endfor %}
</ul>
```

- `{% empty %}`: se ejecuta si la lista está vacía.
- Variables útiles dentro del `for`:

|Variable|Valor|
|---|---|
|`forloop.counter`|Número de vuelta (empieza en 1).|
|`forloop.counter0`|Número de vuelta (empieza en 0).|
|`forloop.first`|`True` en la primera vuelta.|
|`forloop.last`|`True` en la última vuelta.|

Tabla con bucle:

```html
<table>
  <thead>
    <tr><th>Nombre</th><th>Precio</th><th>Stock</th></tr>
  </thead>
  <tbody>
    {% for p in productos %}
      <tr>
        <td>{{ p.nombre }}</td>
        <td>{{ p.precio }}</td>
        <td>{{ p.stock }}</td>
      </tr>
    {% endfor %}
  </tbody>
</table>
```

### 16.5. Condiciones: `if`

```html
{% if producto.stock > 0 %}
  <span>Disponible</span>
{% elif producto.stock == 0 %}
  <span>Agotado</span>
{% else %}
  <span>Sin información</span>
{% endif %}
```

Operadores: `==`, `!=`, `>`, `<`, `>=`, `<=`, `and`, `or`, `not`, `in`.

```html
{% if producto.stock > 0 and producto.precio < 100000 %}
  <span>¡Oferta!</span>
{% endif %}

{% if productos %}
  <p>Hay {{ productos|length }} productos.</p>
{% endif %}
```

Combinar `for` + `if`:

```html
{% for p in productos %}
  {% if p.stock > 0 %}
    <li>{{ p.nombre }}</li>
  {% endif %}
{% endfor %}
```

### 16.6. Filtros útiles

```html
{{ nombre|upper }}                          {# MAYÚSCULAS #}
{{ nombre|lower }}                          {# minúsculas #}
{{ nombre|title }}                          {# Primera Letra Mayúscula #}
{{ descripcion|truncatechars:50 }}          {# corta a 50 caracteres #}
{{ precio|floatformat:2 }}                  {# 2 decimales #}
{{ producto.creado_en|date:"d/m/Y" }}       {# formato de fecha #}
{{ productos|length }}                      {# cantidad de elementos #}
{{ valor|default:"Sin dato" }}              {# si está vacío o None #}
```

### 16.7. Enlaces con `{% url %}`

Usa el `name` de la ruta (así no escribes URLs a mano):

```html
<a href="{% url 'lista_productos' %}">Ver todos</a>

{% for p in productos %}
  <a href="{% url 'detalle_producto' p.id %}">{{ p.nombre }}</a>
{% endfor %}
```

### 16.8. Herencia de templates (base + hijos)

`templates/base.html`:

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>{% block titulo %}Mi tienda{% endblock %}</title>
</head>
<body>
  <nav>
    <a href="{% url 'lista_productos' %}">Inicio</a>
  </nav>

  {% block contenido %}{% endblock %}
</body>
</html>
```

`productos/templates/productos/lista.html`:

```html
{% extends 'base.html' %}

{% block titulo %}Productos{% endblock %}

{% block contenido %}
  <h1>Productos</h1>
  {% for p in productos %}
    <p>{{ p.nombre }}</p>
  {% endfor %}
{% endblock %}
```

### 16.9. Archivos estáticos (CSS, JS, imágenes)

```html
{% load static %}
<link rel="stylesheet" href="{% static 'css/estilos.css' %}">
```

Los archivos van en `productos/static/css/estilos.css` (o en una carpeta `static/` global configurada con `STATICFILES_DIRS`).

## 17. Error 404 (página no encontrada)

### 17.1. `get_object_or_404`

Lo más común. Si el objeto no existe, Django devuelve un 404 automáticamente (en vez de romper con un error 500):

```python
from django.shortcuts import get_object_or_404

def detalle_producto(request, pk):
    producto = get_object_or_404(Producto, pk=pk)
    return render(request, 'productos/detalle.html', {'producto': producto})
```

También acepta filtros: `get_object_or_404(Producto, nombre="Teclado")`.

### 17.2. Lanzar el 404 manualmente

```python
from django.http import Http404

def detalle_producto(request, pk):
    try:
        producto = Producto.objects.get(pk=pk)
    except Producto.DoesNotExist:
        raise Http404("El producto no existe")
    return render(request, 'productos/detalle.html', {'producto': producto})
```

### 17.3. Página 404 personalizada

1. Crea `templates/404.html` (en la carpeta global de templates):

```html
{% extends 'base.html' %}

{% block contenido %}
  <h1>404 - Página no encontrada</h1>
  <p>Lo que buscas no existe.</p>
  <a href="{% url 'lista_productos' %}">Volver al inicio</a>
{% endblock %}
```

2. En `settings.py`:

```python
DEBUG = False
ALLOWED_HOSTS = ['localhost', '127.0.0.1']
```

> [!warning] Con `DEBUG = True` Django muestra su página técnica de error, **no tu 404.html**. Tu página solo aparece con `DEBUG = False`. Con `DEBUG = False` los archivos estáticos no se sirven solos en `runserver` (usa `python manage.py runserver --insecure` solo para probar).

Django busca automáticamente `404.html` (también `403.html` y `500.html`) en la raíz de los templates.

## 18. Autenticación (login) básica

Antes de hablar de permisos, hay que saber **quién es el usuario**.

### 18.1. Objeto `request.user`

En cualquier vista:

```python
request.user                    # el usuario actual
request.user.is_authenticated   # True si inició sesión
request.user.is_staff           # True si puede entrar al admin
request.user.is_superuser       # True si tiene TODOS los permisos
request.user.username
```

En un template (disponible sin pasarlo):

```html
{% if user.is_authenticated %}
  <p>Hola, {{ user.username }}</p>
{% else %}
  <a href="{% url 'login' %}">Iniciar sesión</a>
{% endif %}
```

### 18.2. Login y logout con las vistas de Django

`tienda/urls.py`:

```python
from django.contrib.auth import views as auth_views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('productos/', include('productos.urls')),
    path('login/', auth_views.LoginView.as_view(template_name='login.html'), name='login'),
    path('logout/', auth_views.LogoutView.as_view(), name='logout'),
]
```

`settings.py`:

```python
LOGIN_URL = 'login'                 # a dónde manda si no ha iniciado sesión
LOGIN_REDIRECT_URL = 'lista_productos'   # a dónde va después de iniciar sesión
LOGOUT_REDIRECT_URL = 'login'
```

`templates/login.html`:

```html
<h1>Iniciar sesión</h1>
<form method="post">
  {% csrf_token %}
  {{ form.as_p }}
  <button type="submit">Entrar</button>
</form>
```

> [!warning]
> 
> - `{% csrf_token %}` es **obligatorio** en todo formulario `POST`, si no Django devuelve error 403.
> - En Django 5+ el logout se hace con una petición **POST** (un formulario con botón), no con un enlace normal.

## 19. Permisos: quién puede ver qué

Hay varios niveles. De más simple a más fino:

### 19.1. Solo usuarios con sesión iniciada: `login_required`

```python
from django.contrib.auth.decorators import login_required


@login_required
def lista_productos(request):
    ...
```

Si el usuario no inició sesión, lo manda a `LOGIN_URL`.

### 19.2. Solo staff o superusuario

```python
from django.contrib.auth.decorators import login_required, user_passes_test


@login_required
@user_passes_test(lambda u: u.is_staff)
def panel_interno(request):
    ...
```

`user_passes_test` recibe una función que devuelve `True` o `False`. Si es `False`, redirige al login.

### 19.3. Permisos de modelo (`permission_required`)

Django crea **4 permisos automáticamente** por cada modelo:

|Permiso|Significa|
|---|---|
|`productos.view_producto`|Ver|
|`productos.add_producto`|Crear|
|`productos.change_producto`|Editar|
|`productos.delete_producto`|Eliminar|

Formato: `app.accion_modelo`.

```python
from django.contrib.auth.decorators import permission_required


@permission_required('productos.add_producto', raise_exception=True)
def crear_producto(request):
    ...
```

- Con `raise_exception=True`, si no tiene el permiso devuelve **403 Forbidden**.
- Sin eso, lo redirige al login.
- Un `superuser` tiene todos los permisos automáticamente.

### 19.4. Asignar permisos (desde el admin)

1. Entra a `/admin/`.
2. **Users** → elige un usuario → sección _User permissions_ → marca los permisos.
3. O mejor: **Groups** → crea un grupo (ej. `Vendedores`) → asígnale permisos → agrega usuarios al grupo.

Así se manejan roles: `Vendedores` pueden agregar y editar, `Clientes` solo ven.

Por código:

```python
from django.contrib.auth.models import Group, Permission

grupo = Group.objects.create(name='Vendedores')
permiso = Permission.objects.get(codename='add_producto')
grupo.permissions.add(permiso)
usuario.groups.add(grupo)
```

### 19.5. Permisos en el template

```html
{% if perms.productos.add_producto %}
  <a href="{% url 'crear_producto' %}">Nuevo producto</a>
{% endif %}

{% if user.is_staff %}
  <a href="/admin/">Panel admin</a>
{% endif %}
```

`perms` está disponible automáticamente en los templates.

> [!warning] Ocultar el botón no es seguridad Esconder un botón en el HTML **no protege** nada: alguien puede escribir la URL a mano. Siempre protege también la **vista** con `login_required` / `permission_required`.

### 19.6. Que cada usuario vea solo lo suyo

Requiere un campo `dueno = ForeignKey(User)` en el modelo (ya está en el ejemplo de la sección 9.5).

```python
from django.core.exceptions import PermissionDenied


@login_required
def mis_productos(request):
    productos = Producto.objects.filter(dueno=request.user)
    return render(request, 'productos/lista.html', {'productos': productos})


@login_required
def editar_producto(request, pk):
    producto = get_object_or_404(Producto, pk=pk)
    if producto.dueno != request.user:
        raise PermissionDenied          # devuelve 403
    ...
```

Alternativa: usar `get_object_or_404(Producto, pk=pk, dueno=request.user)` → si no es suyo, da 404 (no revela que existe).

### 19.7. Permisos en vistas basadas en clases

```python
from django.contrib.auth.mixins import LoginRequiredMixin, PermissionRequiredMixin
from django.views.generic import ListView


class ProductoLista(LoginRequiredMixin, ListView):
    model = Producto
    template_name = 'productos/lista.html'
    context_object_name = 'productos'


class ProductoCrear(PermissionRequiredMixin, CreateView):
    permission_required = 'productos.add_producto'
    ...
```

El mixin va **antes** de la clase de vista.

### 19.8. Resumen de permisos

|Quiero...|Uso|
|---|---|
|Que haya iniciado sesión|`@login_required`|
|Solo staff|`@user_passes_test(lambda u: u.is_staff)`|
|Un permiso específico|`@permission_required('app.accion_modelo')`|
|Solo el dueño del registro|Comparar `objeto.dueno != request.user` → `PermissionDenied`|
|Roles|Grupos + permisos en el admin|
|Mostrar/ocultar en HTML|`{% if perms.app.permiso %}`|

## 20. Flujo completo de una petición

```
Usuario
   |
   v
Petición HTTP
   |
   v
tienda/urls.py  →  include('productos.urls')  →  productos/urls.py
   |
   v
Vista (views.py)   ← aquí se revisan los permisos (login_required, etc.)
   |
   v
Modelo (models.py) → Base de datos
   |
   v
Vista arma el contexto {'productos': ...}
   |
   v
Template (.html) con {{ }} y {% %}
   |
   v
Respuesta HTML / JSON  →  Usuario
```

Las vistas no siempre consultan modelos: pueden devolver texto, JSON o HTML directamente.

## 21. Comprobar que todo funciona

```bash
python manage.py check
python manage.py runserver
```

Prueba:

```
http://127.0.0.1:8000/productos/
http://127.0.0.1:8000/productos/1/
http://127.0.0.1:8000/productos/999/       ← debería dar 404
http://127.0.0.1:8000/admin/
```

Si cambiaste modelos, recuerda `makemigrations` y `migrate` antes.

## 22. Comandos importantes de Django

|Comando|Función|
|---|---|
|`python manage.py runserver`|Inicia el servidor de desarrollo.|
|`python manage.py startapp productos`|Crea una aplicación.|
|`python manage.py check`|Comprueba la configuración.|
|`python manage.py makemigrations`|Genera archivos de migración.|
|`python manage.py migrate`|Aplica las migraciones.|
|`python manage.py showmigrations`|Estado de las migraciones.|
|`python manage.py createsuperuser`|Crea un usuario administrador.|
|`python manage.py changepassword usuario`|Cambia la contraseña de un usuario.|
|`python manage.py shell`|Consola interactiva de Django (probar el ORM).|
|`python manage.py collectstatic`|Reúne los archivos estáticos (para producción).|

## 23. Conceptos fundamentales

- **Proyecto:** configuración general (rutas principales, settings, apps registradas).
- **Aplicación (app):** módulo con una funcionalidad (productos, usuarios, pedidos).
- **Modelo:** clase que define la estructura de los datos (una tabla).
- **Migración:** archivo que describe cambios en la estructura de la base de datos.
- **Vista:** función o clase que procesa una petición y devuelve una respuesta.
- **Template:** archivo HTML con lógica de Django (`{{ }}`, `{% %}`).
- **Contexto:** diccionario con los datos que la vista envía al template.
- **Ruta:** relaciona una URL con una vista.
- **Endpoint:** dirección de la app a la que se envía una petición.
- **ORM:** permite consultar la base de datos con Python en vez de SQL.
- **QuerySet:** lista de objetos que devuelve el ORM (`all()`, `filter()`...).
- **Admin:** panel automático para gestionar datos.
- **Permiso:** autorización para hacer una acción (ver, crear, editar, borrar).
- **Grupo:** conjunto de permisos que se asigna a varios usuarios (rol).

## 24. Volver a trabajar en el proyecto

```bash
cd django_aprendizaje
source env/bin/activate
python manage.py runserver
```

Ejecuta los comandos desde la carpeta donde está `manage.py`.

## 25. Resumen del flujo de trabajo

```
1.  Crear el proyecto
2.  Crear una app
3.  Registrar la app en INSTALLED_APPS
4.  Definir los modelos en models.py
5.  makemigrations + migrate
6.  Registrar los modelos en admin.py + createsuperuser
7.  Crear vistas en views.py (consultan el ORM, usan render)
8.  Crear los templates (.html) con for / if
9.  Definir rutas en urls.py de la app
10. Conectar las rutas con include()
11. Proteger vistas con login_required / permission_required
12. Probar (y revisar el 404)
```

---

## 26. Práctica

### Ejercicio resuelto: productos con poco stock

**Objetivo:** una página que liste solo los productos con stock menor a 5, ordenados del que menos tiene al que más, y que muestre "Agotado" si el stock es 0.

**Vista:**

```python
@login_required
def poco_stock(request):
    productos = Producto.objects.filter(stock__lt=5).order_by('stock')
    return render(request, 'productos/poco_stock.html', {'productos': productos})
```

**Ruta (`productos/urls.py`):**

```python
path('poco-stock/', views.poco_stock, name='poco_stock'),
```

**Template (`poco_stock.html`):**

```html
{% extends 'base.html' %}

{% block contenido %}
  <h1>Productos con poco stock</h1>

  <ul>
    {% for p in productos %}
      <li>
        {{ p.nombre }} -
        {% if p.stock == 0 %}
          <strong>Agotado</strong>
        {% else %}
          Quedan {{ p.stock }}
        {% endif %}
      </li>
    {% empty %}
      <li>Todo tiene buen stock 👍</li>
    {% endfor %}
  </ul>
{% endblock %}
```

### Ejercicio propio

Haz una página `/productos/categoria/<int:pk>/` que:

1. Muestre **todos los productos de una categoría** (pista: `categoria.productos.all()` o `filter(categoria=...)`).
2. Devuelva **404** si la categoría no existe (pista: `get_object_or_404`).
3. Solo la pueda ver quien tenga el permiso `productos.view_producto` (pista: `@permission_required(..., raise_exception=True)`).
4. Muestre el nombre de la categoría como título y use `{% empty %}` si no tiene productos.

Prueba con un usuario normal (debe dar 403) y con el superusuario.

---

**Siguiente tema:** formularios (`forms.py` / `ModelForm`) para crear y editar registros desde el navegador, y vistas basadas en clases (`ListView`, `DetailView`, `CreateView`).

## Ver también

- [[MOC-programacion]]
- [[Ruta Python]]
- [[Rutas]]
- [[01-Conceptos de mysql|Conceptos de mysql]]
- [[31-Flash en python (bases)|Flash en python (bases)]]
- [[30-Sockets|Sockets]]
- [[MOC-ciberseguridad]]
