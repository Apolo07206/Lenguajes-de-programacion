---
title: Permisos y propietarios
area: programacion
tipo: fundamento
tags: [programacion, bash, linux, permisos]
created: 2026-10-12
updated: 2026-11-01
related: ["[[MOC-programacion]]", "[[Ruta Bash]]", "[[Comandos de Linux]]", "[[Permisos, Servicios y Logs en Linux]]"]
---

# Permisos y propietarios en Linux: `chmod` y `chown`

En Linux, cada archivo y directorio tiene un propietario, un grupo asociado y un conjunto de permisos que determinan quién puede acceder a él y qué acciones puede realizar.

Para administrar estos aspectos se utilizan principalmente dos comandos:

- `**chown**` **(Change Owner):** permite cambiar el usuario propietario y el grupo de un archivo o directorio.
    
- `**chmod**` **(Change Mode):** permite modificar los permisos de acceso de un archivo o directorio.
    

La diferencia principal es:

- `chown` responde a la pregunta: **¿Quién es el propietario?**
    
- `chmod` responde a la pregunta: **¿Qué acciones están permitidas?**
    

---

## 1. `chown`: cambiar el propietario

El comando `chown` permite asignar un archivo o directorio a un usuario y a un grupo determinados.

En Linux, cada archivo tiene un propietario y un grupo propietario. Ambos se utilizan para administrar los permisos de acceso.

### Sintaxis

```
sudo chown [usuario]:[grupo] [archivo_o_directorio]
```

- `sudo`: ejecuta el comando con privilegios administrativos. Normalmente se necesita para cambiar la propiedad de archivos que pertenecen a otros usuarios.
    
- `usuario`: nombre del nuevo propietario.
    
- `grupo`: nombre del nuevo grupo propietario.
    
- `archivo_o_directorio`: elemento al que se le cambiará la propiedad.
    

### Ejemplos prácticos

**Cambiar solamente el usuario propietario:**

```
sudo chown carlos examen.txt
```

El usuario `carlos` pasa a ser el propietario de `examen.txt`. El grupo propietario no cambia.

**Cambiar el usuario y el grupo al mismo tiempo:**

```
sudo chown carlos:profesores examen.txt
```

El usuario propietario será `carlos` y el grupo propietario será `profesores`.

**Cambiar solamente el grupo propietario:**

```
sudo chown :profesores examen.txt
```

El archivo seguirá perteneciendo al mismo usuario, pero pasará a formar parte del grupo `profesores`.

**Cambiar la propiedad de una carpeta y de todo su contenido:**

```
sudo chown -R carlos:profesores /var/www/html/
```

La opción `-R` significa recursivo. Aplica el cambio al directorio indicado y a los archivos y subdirectorios que contiene.

> [!WARNING] Precaución con `-R`  
> Antes de utilizar `chown -R`, verifica cuidadosamente la ruta. Un cambio recursivo sobre un directorio incorrecto puede modificar la propiedad de muchos archivos importantes del sistema.

---

## 2. `chmod`: cambiar los permisos

El comando `chmod` permite establecer qué acciones pueden realizar los usuarios sobre un archivo o directorio.

Linux organiza los permisos en tres categorías de usuarios y tres tipos de permisos.

### 2.1. Categorías de usuarios

Los permisos se distribuyen entre tres categorías:

|Categoría|Significado|Descripción|
|---|---|---|
|`u`|User|Usuario propietario del archivo.|
|`g`|Group|Usuarios que pertenecen al grupo propietario.|
|`o`|Others|Todos los demás usuarios del sistema.|
|`a`|All|Representa a todas las categorías: propietario, grupo y otros.|

Por ejemplo, si un archivo pertenece al usuario `carlos` y al grupo `profesores`:

- `u`: representa a `carlos`.
    
- `g`: representa a los miembros de `profesores`.
    
- `o`: representa a los usuarios que no son `carlos` ni pertenecen al grupo `profesores`.
    

### 2.2. Tipos de permisos

Existen tres permisos básicos:

|Permiso|Letra|Descripción|
|---|---|---|
|Lectura|`r`|Permite consultar el contenido de un archivo.|
|Escritura|`w`|Permite modificar el contenido de un archivo.|
|Ejecución|`x`|Permite ejecutar un archivo si es ejecutable.|

El significado de estos permisos cambia ligeramente dependiendo de si se aplican a un archivo o a un directorio.

#### Permisos en archivos

- `r`: permite leer el contenido del archivo.
    
- `w`: permite modificar el contenido del archivo.
    
- `x`: permite ejecutar el archivo, siempre que su contenido y formato permitan hacerlo.
    

#### Permisos en directorios

- `r`: permite listar los nombres de los elementos que contiene el directorio.
    
- `w`: permite crear, eliminar y renombrar elementos dentro del directorio, normalmente junto con el permiso `x`.
    
- `x`: permite acceder al directorio y atravesarlo para llegar a sus archivos o subdirectorios.
    

> [!IMPORTANT] Diferencia entre archivos y directorios  
> En un archivo, `x` permite ejecutarlo. En un directorio, `x` permite acceder a él o atravesarlo.
> 
> Tener permiso de lectura sobre un directorio no garantiza que puedas acceder a sus archivos. Para llegar a ellos también necesitas el permiso de ejecución (`x`) en el directorio y en los directorios superiores de la ruta.

---

## 3. Métodos para modificar permisos con `chmod`

Existen dos formas habituales de modificar permisos: la notación simbólica y la notación octal.

### 3.1. Notación simbólica

La notación simbólica utiliza letras para indicar a quién se aplicará el cambio y qué permiso se modificará.

Los operadores principales son:

|Operador|Función|
|---|---|
|`+`|Agrega un permiso.|
|`-`|Elimina un permiso.|
|`=`|Asigna exactamente los permisos indicados.|

La estructura general es:

```
chmod [categoría][operador][permiso] archivo
```

#### Ejemplos

**Dar permiso de ejecución al propietario:**

```
chmod u+x script.sh
```

El propietario del archivo obtiene el permiso de ejecución.

**Quitar el permiso de escritura al grupo:**

```
chmod g-w documento.txt
```

Los miembros del grupo propietario dejan de tener permiso para modificar el archivo.

**Asignar únicamente permiso de lectura a otros usuarios:**

```
chmod o=r reporte.pdf
```

Los demás usuarios tendrán solamente permiso de lectura. Cualquier otro permiso que tuvieran en esa categoría será eliminado.

**Quitar el permiso de ejecución a todas las categorías:**

```
chmod a-x script.sh
```

Se elimina el permiso de ejecución del propietario, del grupo y de los demás usuarios.

### 3.2. Notación octal o numérica

La notación octal utiliza números para representar los permisos. Es una de las formas más utilizadas para configurar permisos en Linux.

Cada permiso tiene un valor:

| Permiso      | Letra | Valor |
| ------------ | ----- | ----- |
| Lectura      | `r`   | 4     |
| Escritura    | `w`   | 2     |
| Ejecución    | `x`   | 1     |
| Sin permisos | `-`   | 0     |

Para obtener un permiso combinado, se suman los valores correspondientes.

#### Combinaciones de permisos

| Valor | Permisos | Significado                     |
| ----- | -------- | ------------------------------- |
| `0`   | `---`    | Sin permisos.                   |
| `1`   | `--x`    | Solo ejecución.                 |
| `2`   | `-w-`    | Solo escritura.                 |
| `3`   | `-wx`    | Escritura y ejecución.          |
| `4`   | `r--`    | Solo lectura.                   |
| `5`   | `r-x`    | Lectura y ejecución.            |
| `6`   | `rw-`    | Lectura y escritura.            |
| `7`   | `rwx`    | Lectura, escritura y ejecución. |

#### Estructura de los tres dígitos

Un permiso como `755` tiene tres dígitos. Cada uno representa una categoría de usuarios:

```
chmod 755 script.sh
      │││
      ││└── Otros (o):       5 = r-x
      │└─── Grupo (g):       5 = r-x
      └──── Propietario (u): 7 = rwx
```

Esto significa:

- Propietario (`7`): puede leer, escribir y ejecutar.
    
- Grupo (`5`): puede leer y ejecutar.
    
- Otros (`5`): pueden leer y ejecutar.
    

Cada dígito se calcula sumando los valores de los permisos correspondientes.

Por ejemplo:

```
7 = 4 + 2 + 1 = rwx
5 = 4 + 0 + 1 = r-x
6 = 4 + 2 + 0 = rw-
```

### 3.3. Ejemplos de permisos habituales

**Permiso** `**755**`**:**

```
chmod 755 script.sh
```

|Categoría|Permisos|
|---|---|
|Propietario|`rwx`|
|Grupo|`r-x`|
|Otros|`r-x`|

El propietario puede leer, modificar y ejecutar el archivo. El grupo y los demás usuarios pueden leerlo y ejecutarlo.

Se utiliza habitualmente en scripts ejecutables y directorios a los que otros usuarios necesitan acceder.

**Permiso** `**644**`**:**

```
chmod 644 documento.txt
```

|Categoría|Permisos|
|---|---|
|Propietario|`rw-`|
|Grupo|`r--`|
|Otros|`r--`|

El propietario puede leer y modificar el archivo. Los demás usuarios solamente pueden leerlo.

Es habitual en documentos, archivos de configuración y archivos HTML que no necesitan permisos de ejecución.

**Permiso** `**700**`**:**

```
chmod 700 archivo_privado
```

|Categoría|Permisos|
|---|---|
|Propietario|`rwx`|
|Grupo|`---`|
|Otros|`---`|

Solamente el propietario tiene permisos de lectura, escritura y ejecución.

Puede utilizarse en directorios privados, como `~/.ssh`.

**Permiso** `**600**`**:**

```
chmod 600 clave_privada
```

|Categoría|Permisos|
|---|---|
|Propietario|`rw-`|
|Grupo|`---`|
|Otros|`---`|

Solamente el propietario puede leer y modificar el archivo. Es una configuración habitual para claves privadas SSH, que no necesitan permiso de ejecución.

**Permiso** `**777**`**:**

```
chmod 777 archivo
```

|Categoría|Permisos|
|---|---|
|Propietario|`rwx`|
|Grupo|`rwx`|
|Otros|`rwx`|

Todos los usuarios tienen permisos de lectura, escritura y ejecución.

> [!WARNING] Riesgo de utilizar `777`  
> No se recomienda utilizar `777` en sistemas reales sin una razón muy específica. Cualquier usuario puede modificar el archivo y, si es ejecutable, también podría ejecutar código que haya introducido.
> 
> Es preferible asignar únicamente los permisos necesarios para cada usuario o grupo.

---

## 4. Cómo consultar los permisos con `ls -l`

Para comprobar los permisos, el propietario y el grupo de un archivo se utiliza:

```
ls -l mi_script.sh
```

Una salida de ejemplo sería:

```
-rwxr-xr-x 1 carlos profesores 1024 Oct 31 18:00 mi_script.sh
```

### Anatomía de la salida

```
-rwxr-xr-x 1 carlos profesores 1024 Oct 31 18:00 mi_script.sh
│└─┬─┘└─┬─┘└─┬─┘
│  │    │    └── Permisos para otros
│  │    └─────── Permisos para el grupo
│  └──────────── Permisos para el propietario
└─────────────── Tipo de archivo
```

La salida también incluye información adicional:

|Elemento|Significado|
|---|---|
|`-rwxr-xr-x`|Tipo de archivo y permisos.|
|`1`|Número de enlaces físicos.|
|`carlos`|Usuario propietario.|
|`profesores`|Grupo propietario.|
|`1024`|Tamaño del archivo en bytes.|
|`Oct 31 18:00`|Fecha y hora de la última modificación.|
|`mi_script.sh`|Nombre del archivo.|

### Cómo interpretar los primeros diez caracteres

El primer carácter indica el tipo de elemento:

|Carácter|Tipo|
|---|---|
|`-`|Archivo regular.|
|`d`|Directorio.|
|`l`|Enlace simbólico.|

Los siguientes nueve caracteres representan los permisos:

```
-rwxr-xr--
 │  │  │
 │  │  └── Otros:       r--
 │  └───── Grupo:       r-x
 └──────── Propietario: rwx
```

En este ejemplo:

- El propietario tiene `rwx`: puede leer, escribir y ejecutar.
    
- El grupo tiene `r-x`: puede leer y ejecutar.
    
- Los demás tienen `r--`: solamente pueden leer.
    

También puedes consultar los permisos de todos los elementos de un directorio:

```
ls -l
```

Para incluir archivos ocultos:

```
ls -la
```

---

## 5. Diferencias entre `chmod` y `chown`

Aunque ambos comandos se relacionan con la administración de permisos, cumplen funciones diferentes.

|Característica|`chmod`|`chown`|
|---|---|---|
|Nombre|Change Mode|Change Owner|
|Función|Modifica los permisos.|Modifica el propietario y el grupo.|
|Trabaja con|Permisos `r`, `w` y `x`.|Usuarios y grupos.|
|Ejemplo|`chmod 755 archivo`|`chown carlos:profesores archivo`|
|Pregunta que responde|¿Qué puede hacer cada usuario?|¿A quién pertenece el archivo?|

### ¿Cuándo utilizar cada uno?

Utiliza `chown` cuando necesites cambiar el propietario o el grupo de un archivo o directorio.

Por ejemplo, cuando un archivo pertenece a `root` y necesitas asignarlo a otro usuario.

```
sudo chown carlos documento.txt
```

Utiliza `chmod` cuando necesites modificar lo que los usuarios pueden hacer con un archivo o directorio.

Por ejemplo, cuando necesitas permitir que un script pueda ejecutarse:

```
chmod u+x script.sh
```

Recuerda que no necesitas cambiar el propietario para modificar los permisos. Son operaciones independientes.

---

## 6. Comandos adicionales útiles

**Consultar el usuario actual:**

```
whoami
```

**Consultar los grupos a los que pertenece el usuario:**

```
groups
```

**Consultar los grupos y el identificador del usuario:**

```
id
```

**Consultar los permisos de un archivo:**

```
ls -l archivo.txt
```

**Consultar los permisos de un directorio:**

```
ls -ld directorio/
```

**Cambiar los permisos de un directorio y su contenido:**

```
chmod -R 755 directorio/
```

La opción `-R` aplica los cambios recursivamente. Ten cuidado: aplicar los mismos permisos a archivos y directorios puede ser incorrecto, porque no todos necesitan permisos de ejecución.

---

## 7. Resumen

|Necesidad|Comando|
|---|---|
|Cambiar el propietario|`chown`|
|Cambiar el grupo propietario|`chown`|
|Cambiar propietario y grupo|`chown`|
|Dar permiso de ejecución|`chmod`|
|Quitar permiso de escritura|`chmod`|
|Asignar permisos específicos|`chmod`|
|Consultar permisos y propietario|`ls -l`|

> [!TIP] Regla para recordar
> 
> - `chown`: determina quién es el propietario del archivo.
>     
> - `chmod`: determina qué puede hacer cada categoría de usuarios con el archivo.
>     
> - `ls -l`: permite consultar los permisos, el propietario y el grupo.
>     

**Ejemplo práctico:**

Imagina que tienes un archivo llamado `script.sh` que pertenece a `root` y quieres que el usuario `carlos` sea su propietario y pueda ejecutarlo:

```
# 1. Cambiar el propietario
sudo chown carlos script.sh

# 2. Dar permiso de ejecución al propietario
chmod u+x script.sh

# 3. Comprobar los cambios
ls -l script.sh
```

Con estos tres pasos cambias quién es el propietario, modificas sus permisos y verificas el resultado.

---

## Ver también

- [[MOC-programacion]]
- [[Ruta Bash]]
- [[Comandos de Linux]]
- [[Permisos, Servicios y Logs en Linux]]
- [[08-Comandos básicos para ciberseguridad en Bash]]
