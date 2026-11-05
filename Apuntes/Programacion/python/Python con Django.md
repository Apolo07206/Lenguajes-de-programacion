---
title: Python con Django
area: programacion
tipo: lenguaje
tags: [programacion, python, django, web]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Rutas]]", "[[Conceptos de mysql]]", "[[Flash en python (bases)]]", "[[Sockets]]"]
---

# Python con Django

## 1. ¿Qué es Django?

Django es un framework de Python que permite desarrollar aplicaciones web de forma rápida, segura y organizada.

Entre sus funcionalidades están:

- Manejo de URLs y peticiones HTTP.
    
- Conexión con bases de datos.
    
- Gestión de usuarios y autenticación.
    
- Formularios.
    
- Generación de páginas HTML.
    
- Creación de APIs y endpoints.
    
- Administración de datos mediante modelos y migraciones.
    

## 2. Crear la carpeta del proyecto

```
mkdir django_aprendizaje
cd django_aprendizaje
```

- `mkdir`: crea una carpeta.
    
- `cd`: entra en una carpeta.
    

## 3. Crear el entorno virtual

```
python3 -m venv env
```

Crea un entorno virtual llamado `env` para instalar las dependencias del proyecto sin mezclarlas con las de otros proyectos.

### Activar el entorno virtual

```
source env/bin/activate
```

Al activarlo, aparecerá `(env)` al inicio de la línea de comandos.

## 4. Instalar Django

```
pip install django
```

Instala Django dentro del entorno virtual activo.

### Comprobar la instalación

```
django-admin --version
```

Muestra la versión de Django instalada.

## 5. Crear el proyecto

```
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

```
python manage.py runserver
```

Inicia el servidor de desarrollo de Django.

Abre en el navegador:

```
http://127.0.0.1:8000/
```

También puedes utilizar:

```
http://localhost:8000/
```

Si todo funciona, aparecerá la página de bienvenida de Django.

Para detener el servidor, presiona `Ctrl + C` en la terminal.

## 7. Crear una aplicación (app)

Un proyecto Django puede contener varias aplicaciones. Cada app se encarga de una funcionalidad específica.

Por ejemplo:

- `productos`: administra los productos.
    
- `usuarios`: gestiona los usuarios.
    
- `pedidos`: administra los pedidos.
    

Para crear una app llamada `productos`:

```
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

- `admin.py`: permite configurar cómo se muestran los modelos en el panel administrativo.
    
- `apps.py`: contiene la configuración de la aplicación.
    
- `models.py`: define los modelos que representan los datos.
    
- `tests.py`: permite escribir pruebas automatizadas.
    
- `views.py`: contiene las vistas que procesan las peticiones.
    
- `migrations/`: almacena los archivos de migración de la base de datos.
    

**Importante:** Django no crea automáticamente un archivo `urls.py` dentro de cada app. Debes crearlo manualmente cuando quieras organizar las rutas de esa aplicación.

## 8. Registrar la aplicación en el proyecto

Crear una app no es suficiente. También debes registrarla en la configuración del proyecto principal.

Abre el archivo:

```
tienda/settings.py
```

Busca la lista `INSTALLED_APPS` y añade la configuración de la aplicación:

```
INSTALLED_APPS = [
    # Aplicaciones incluidas con Django
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    # Aplicaciones propias
    'productos.apps.ProductosConfig',
]
```

`productos.apps.ProductosConfig` hace referencia a la clase de configuración definida en `productos/apps.py`.

Su estructura habitual es:

```
from django.apps import AppConfig


class ProductosConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'productos'
```

Registrar la aplicación permite que Django reconozca sus modelos y otras funcionalidades.

## 9. Crear modelos en Django

Los modelos representan los datos que maneja una aplicación. Normalmente, cada modelo se corresponde con una tabla de la base de datos.

Los modelos se definen como clases que heredan de `models.Model`. Sus atributos representan campos de la tabla.

Abre:

```
productos/models.py
```

### Ejemplo: modelo Producto

```
from django.db import models


class Producto(models.Model):
    nombre = models.CharField(max_length=100)
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.PositiveIntegerField(default=0)
    creado_en = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.nombre
```

### Explicación de los campos

- `CharField`: almacena texto de longitud limitada.
    
- `max_length=100`: establece una longitud máxima de 100 caracteres.
    
- `DecimalField`: almacena números decimales, útil para precios.
    
- `max_digits=10`: permite hasta diez dígitos en total.
    
- `decimal_places=2`: permite dos cifras decimales.
    
- `PositiveIntegerField`: almacena números enteros no negativos.
    
- `default=0`: establece cero como valor predeterminado.
    
- `DateTimeField`: almacena fechas y horas.
    
- `auto_now_add=True`: registra automáticamente la fecha y hora de creación.
    
- `__str__()`: define la representación legible del objeto.
    

Por ejemplo, un registro podría representar lo siguiente:

|Campo|Valor|
|---|---|
|nombre|Teclado mecánico|
|precio|150000.00|
|stock|12|
|creado_en|Fecha y hora de creación|

Los nombres y tipos de los campos determinan cómo se representa la información en la base de datos. Django se encarga de traducir los modelos al esquema correspondiente para el motor de base de datos configurado.

## 10. Crear y aplicar migraciones

Cuando creas o modificas un modelo, Django necesita registrar esos cambios y aplicarlos a la base de datos.

Para ello se utilizan las migraciones.

### 10.1. Crear las migraciones

```
python manage.py makemigrations
```

También puedes indicar la app específica:

```
python manage.py makemigrations productos
```

Este comando detecta los cambios realizados en los modelos y genera archivos de migración dentro de la carpeta `productos/migrations/`.

Por ejemplo:

```
productos/
└── migrations/
    ├── __init__.py
    └── 0001_initial.py
```

El archivo `0001_initial.py` contiene las operaciones necesarias para crear inicialmente el modelo.

**Importante:** `makemigrations` crea el archivo que describe los cambios, pero todavía no los aplica a la base de datos.

### 10.2. Aplicar las migraciones

```
python manage.py migrate
```

Este comando aplica las migraciones pendientes a la base de datos configurada.

Django utiliza SQLite de forma predeterminada en un proyecto nuevo, salvo que cambies la configuración.

### Diferencia entre ambos comandos

|Comando|Función|
|---|---|
|`makemigrations`|Genera los archivos que describen los cambios de los modelos.|
|`migrate`|Aplica esos cambios a la base de datos.|
|`showmigrations`|Muestra las migraciones y su estado.|
|`sqlmigrate productos 0001`|Muestra el SQL asociado a una migración específica.|

### Flujo de trabajo

Cuando crees o modifiques un modelo, normalmente seguirás estos pasos:

```
python manage.py makemigrations
python manage.py migrate
```

Si solo modificas el código de una vista o una ruta, no necesitas crear migraciones, a menos que también hayas cambiado los modelos.

## 11. Crear vistas y endpoints

Las vistas reciben las peticiones HTTP y deciden qué respuesta devolver.

Un endpoint es una dirección de una aplicación web a la que se puede enviar una petición para obtener información o ejecutar una operación.

Por ejemplo:

```
GET /productos/
GET /productos/saludo/
```

Estos dos endpoints podrían devolver la lista de productos y un mensaje de bienvenida, respectivamente.

### 11.1. Crear la primera vista

Abre `productos/views.py` y escribe:

```
from django.http import HttpResponse, JsonResponse


def saludo(request):
    return HttpResponse("Hola, Django!")


def lista_productos(request):
    productos = [
        {"id": 1, "nombre": "Teclado", "precio": 150000},
        {"id": 2, "nombre": "Ratón", "precio": 60000},
    ]

    return JsonResponse(productos, safe=False)
```

### Explicación

- `request`: contiene información sobre la petición recibida.
    
- `HttpResponse`: devuelve una respuesta HTTP, en este caso texto.
    
- `JsonResponse`: devuelve datos en formato JSON.
    
- `safe=False`: permite devolver una lista como respuesta JSON. Por defecto, `JsonResponse` espera un diccionario.
    

La función `lista_productos` utiliza datos de ejemplo escritos directamente en Python. Todavía no consulta la base de datos.

## 12. Crear las rutas de la aplicación

Para mantener el código organizado, cada app puede tener su propio archivo `urls.py`.

Crea manualmente el archivo:

```
productos/urls.py
```

Añade lo siguiente:

```
from django.urls import path
from . import views


urlpatterns = [
    path('', views.lista_productos, name='lista_productos'),
    path('saludo/', views.saludo, name='saludo'),
]
```

### Explicación

- `path()`: define una ruta y la función que debe atenderla.
    
- `''`: representa la ruta raíz de esta app.
    
- `views.lista_productos`: indica qué vista se ejecutará.
    
- `name`: permite identificar la ruta por un nombre dentro de Django.
    
- `from . import views`: importa las vistas de la misma aplicación.
    

En este archivo no se escribe el prefijo `productos/`. Ese prefijo se añadirá desde el proyecto principal.

## 13. Conectar las rutas de la app con el proyecto principal

Abre el archivo:

```
tienda/urls.py
```

Configúralo de la siguiente manera:

```
from django.contrib import admin
from django.urls import path, include


urlpatterns = [
    path('admin/', admin.site.urls),
    path('productos/', include('productos.urls')),
]
```

### ¿Qué hace `include()`?

La función `include()` permite delegar el manejo de un conjunto de rutas a otro archivo `urls.py`.

En este caso:

```
path('productos/', include('productos.urls'))
```

Le indica a Django que las rutas que comiencen por `productos/` deben continuar resolviéndose en `productos/urls.py`.

Así, el proyecto principal no necesita contener todas las rutas de todas las aplicaciones.

### ¿Cómo se forman las URLs finales?

La URL final resulta de combinar el prefijo definido en el proyecto principal con la ruta definida en la app.

|Proyecto principal|Ruta de la app|URL final|
|---|---|---|
|`productos/`|`''`|`/productos/`|
|`productos/`|`saludo/`|`/productos/saludo/`|

Si accedes a:

```
http://127.0.0.1:8000/productos/
```

Se ejecutará la vista `lista_productos`.

Si accedes a:

```
http://127.0.0.1:8000/productos/saludo/
```

Se ejecutará la vista `saludo`.

### ¿Por qué organizar las rutas de esta manera?

Imagina que el proyecto tiene tres aplicaciones:

```
tienda/
├── urls.py
├── productos/
│   └── urls.py
├── usuarios/
│   └── urls.py
└── pedidos/
    └── urls.py
```

Cada app puede gestionar sus propias rutas, mientras que el archivo principal conecta las diferentes aplicaciones.

Esto permite mantener un código más modular, fácil de mantener y ampliar.

## 14. Flujo completo de una petición

Cuando un usuario accede a una URL, Django sigue un flujo parecido al siguiente:

```
Usuario
   |
   v
Petición HTTP
   |
   v
tienda/urls.py
   |
   v
include('productos.urls')
   |
   v
productos/urls.py
   |
   v
productos/views.py
   |
   v
Respuesta HTTP o JSON
   |
   v
Usuario
```

Cuando una vista necesita información almacenada en la base de datos, el flujo puede incluir los modelos:

```
Petición HTTP
      |
      v
URLs del proyecto
      |
      v
URLs de la app
      |
      v
Vista (views.py)
      |
      v
Modelo (models.py)
      |
      v
Base de datos
      |
      v
Vista
      |
      v
Respuesta al usuario
```

Las vistas no siempre necesitan consultar modelos. Pueden devolver texto, JSON, HTML u otras respuestas directamente.

## 15. Comprobar que todo funciona

Desde la carpeta donde se encuentra `manage.py`, ejecuta:

```
python manage.py check
```

Este comando comprueba distintos aspectos de la configuración del proyecto.

Después, inicia el servidor:

```
python manage.py runserver
```

Prueba los siguientes endpoints en el navegador:

```
http://127.0.0.1:8000/productos/
http://127.0.0.1:8000/productos/saludo/
```

La primera URL devolverá una lista JSON de productos de ejemplo. La segunda mostrará el mensaje `Hola, Django!`.

Si cambiaste los modelos, recuerda ejecutar las migraciones antes de utilizar esos modelos para consultar o guardar datos.

## 16. Comandos importantes de Django

|Comando|Función|
|---|---|
|`python manage.py runserver`|Inicia el servidor de desarrollo.|
|`python manage.py startapp productos`|Crea una aplicación.|
|`python manage.py check`|Comprueba la configuración del proyecto.|
|`python manage.py makemigrations`|Genera archivos de migración.|
|`python manage.py migrate`|Aplica las migraciones pendientes.|
|`python manage.py showmigrations`|Muestra el estado de las migraciones.|
|`python manage.py createsuperuser`|Crea un usuario administrador.|
|`python manage.py shell`|Abre una consola interactiva de Django.|

## 17. Conceptos fundamentales

### Proyecto

Es la configuración general de la aplicación web. Gestiona aspectos como las rutas principales, los ajustes y las aplicaciones registradas.

### Aplicación (app)

Es un módulo que agrupa funcionalidades específicas. Por ejemplo, productos, usuarios o pedidos.

### Modelo

Es una clase que define la estructura de los datos que maneja una aplicación y permite interactuar con ellos mediante el ORM de Django.

### Migración

Es un archivo que describe cambios en la estructura de los datos. Permite mantener sincronizados los modelos y el esquema de la base de datos.

### Vista

Es una función o clase que procesa una petición y devuelve una respuesta.

### Ruta

Relaciona una URL con una vista que debe procesar la petición.

### Endpoint

Es una dirección de la aplicación a través de la cual se puede acceder a una funcionalidad mediante una petición HTTP.

### API

Es una interfaz que permite que distintos programas se comuniquen. En una API web, los endpoints suelen recibir peticiones HTTP y devolver datos, frecuentemente en formato JSON.

### ORM

El ORM de Django permite consultar y modificar datos usando clases y objetos de Python, sin tener que escribir manualmente SQL para cada operación habitual.

## 18. Nota importante: volver a trabajar en el proyecto

Cada vez que vuelvas a trabajar en el proyecto, abre una terminal y entra en la carpeta correspondiente:

```
cd django_aprendizaje
source env/bin/activate
```

Después, inicia el servidor cuando lo necesites:

```
python manage.py runserver
```

Recuerda ejecutar los comandos de administración desde la carpeta donde se encuentra `manage.py`.

## 19. Resumen del flujo de trabajo

```
1. Crear el proyecto
        |
        v
2. Crear una app
        |
        v
3. Registrar la app en INSTALLED_APPS
        |
        v
4. Definir los modelos en models.py
        |
        v
5. Crear migraciones
        |
        v
6. Aplicar migraciones
        |
        v
7. Crear vistas en views.py
        |
        v
8. Definir rutas en urls.py de la app
        |
        v
9. Conectar las rutas con include()
        |
        v
10. Probar los endpoints
```

**Siguiente tema:** consultar, crear, actualizar y eliminar registros de la base de datos mediante el ORM de Django, conectando los modelos con las vistas y los endpoints.

## Ver también

- [[MOC-programacion]]
- [[Ruta Python]]
- [[Rutas]]
- [[Conceptos de mysql]]
- [[Flash en python (bases)]]
- [[Sockets]]
- [[MOC-ciberseguridad]]