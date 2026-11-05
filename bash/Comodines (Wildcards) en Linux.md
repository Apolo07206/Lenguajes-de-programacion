---
title: Comodines (Wildcards) en Linux
area: programacion
tipo: fundamento
tags: [programacion, bash, linux, wildcards]
created: 2026-10-12
updated: 2026-11-01
related: ["[[MOC-programacion]]", "[[Ruta Bash]]", "[[Comandos de Linux]]", "[[01-Introducción a Bash]]"]
---

# Comodines (Wildcards) en Linux

Los **comodines**, también conocidos como _wildcards_ o patrones glob, son caracteres especiales que permiten seleccionar archivos y directorios utilizando coincidencias en sus nombres.

En lugar de escribir el nombre completo de cada archivo, podemos utilizar patrones para encontrar varios archivos que cumplan determinadas condiciones.

Son especialmente útiles al trabajar con comandos como `ls`, `cp`, `mv`, `rm` y `find`.

> [!IMPORTANT] Concepto clave  
> Los comodines normalmente son interpretados por el shell (por ejemplo, Bash) antes de ejecutar el comando. Esto significa que Bash sustituye el patrón por los nombres de archivos que coinciden con él.

---

## 1. Asterisco `*`

Representa **cualquier cantidad de caracteres**, incluyendo cero caracteres.

Es uno de los comodines más utilizados.

|Patrón|Coincidencias de ejemplo|
|---|---|
|`*.txt`|`notas.txt`, `archivo.txt`|
|`documento*`|`documento1`, `documento_final`|
|`*linux*`|`linux.txt`, `apuntes_linux.pdf`|
|`foto*.*`|`foto1.jpg`, `foto_vacaciones.png`|
|`*.pdf`|Todos los archivos PDF del directorio actual|

**Ejemplos prácticos:**

```bash
# Mostrar todos los archivos .txt
ls *.txt

# Mostrar archivos que comiencen por "documento"
ls documento*

# Mostrar nombres que contengan "linux"
ls *linux*

# Copiar todos los archivos PDF a otra carpeta
cp *.pdf documentos/
```

---

## 2. Signo de interrogación `?`

Representa **exactamente un carácter**.

A diferencia del asterisco, no puede representar cero ni varios caracteres.

|Patrón|Coincidencias de ejemplo|
|---|---|
|`archivo?.txt`|`archivo1.txt`, `archivoA.txt`|
|`foto??.jpg`|`foto01.jpg`, `fotoAB.jpg`|
|`?.txt`|`a.txt`, `1.txt`|
|`linux?.pdf`|`linux1.pdf`, `linuxA.pdf`|

**Ejemplos prácticos:**

```bash
# Archivos con un solo carácter después de "archivo"
ls archivo?.txt

# Archivos que tengan dos caracteres antes de .jpg
ls foto??.jpg
```

---

## 3. Corchetes `[]`

Permiten especificar un conjunto de caracteres posibles para una posición determinada.

El patrón coincide cuando el carácter que ocupa esa posición pertenece al conjunto indicado.

### A. Caracteres específicos

```bash
ls archivo[123].txt
```

Puede encontrar:

- archivo1.txt
    
- archivo2.txt
    
- archivo3.txt
    

No encontrará `archivo4.txt`.

### B. Rangos de caracteres

Podemos utilizar guiones para establecer rangos.

|Patrón|Significado|
|---|---|
|`[0-9]`|Cualquier dígito del 0 al 9|
|`[a-z]`|Una letra minúscula|
|`[A-Z]`|Una letra mayúscula|
|`[a-f]`|Una letra entre a y f|
|`[0-9a-z]`|Un dígito o una letra minúscula|

**Ejemplos:**

```bash
# Archivos terminados en un número
ls archivo[0-9].txt

# Archivos que comiencen con una letra minúscula
ls [a-z]*

# Archivos que comiencen por una letra mayúscula
ls [A-Z]*
```

### C. Negación `[! ]`

Dentro de los corchetes, `!` al principio significa que queremos excluir los caracteres indicados.

```bash
ls archivo[!0-9].txt
```

Coincide con nombres como:

- archivoA.txt
    
- archivoB.txt
    

No coincide con:

- archivo1.txt
    
- archivo2.txt
    

> [!NOTE]  
> En Bash, también existe la sintaxis `[^...]` para negar un conjunto, aunque `[!...]` es la forma tradicional de los patrones glob del shell.

---

## 4. Comodines combinados

Podemos combinar varios comodines para crear patrones más específicos.

|Patrón|Significado|
|---|---|
|`*.txt`|Cualquier nombre terminado en .txt|
|`archivo?.*`|archivo + un carácter + cualquier extensión|
|`foto[0-9].jpg`|Foto seguida de un número|
|`backup*.[tz][i][p]`|Patrón con caracteres específicos en la extensión|
|`[A-Z]*.pdf`|PDF cuyo nombre comienza con mayúscula|

**Ejemplos:**

```bash
# Archivos llamados documento, seguidos de un número
ls documento[0-9].pdf

# Fotos numeradas de dos dígitos
ls foto[0-9][0-9].jpg

# Archivos que comiencen por una letra mayúscula
ls [A-Z]*.txt
```

---

## 5. Llaves `{}`: expansión de llaves

Las llaves permiten generar varias cadenas a partir de una sola expresión.

**Importante:** técnicamente, esto no es un comodín glob, sino una característica de expansión de Bash.

### A. Crear varios directorios

```bash
mkdir {documentos,imagenes,backups}
```

Es equivalente a ejecutar:

```bash
mkdir documentos imagenes backups
```

### B. Crear varios archivos

```bash
touch archivo{1,2,3}.txt
```

Genera:

- archivo1.txt
    
- archivo2.txt
    
- archivo3.txt
    

### C. Utilizar rangos

```bash
touch archivo{1..5}.txt
```

Genera archivos desde `archivo1.txt` hasta `archivo5.txt`.

También podemos utilizar letras:

```bash
touch letra{a..e}.txt
```

---

## 6. El doble asterisco `**`

En Bash, el doble asterisco puede utilizarse para buscar coincidencias en directorios y subdirectorios de forma recursiva.

Sin embargo, esta característica requiere activar `globstar`.

```bash
# Activar la expansión recursiva
shopt -s globstar

# Buscar archivos .txt en el directorio actual y sus subdirectorios
ls **/*.txt
```

Para desactivarla:

```bash
shopt -u globstar
```

> [!WARNING]  
> `**` no se comporta como un patrón recursivo por defecto en todas las configuraciones o shells. En Bash debes activar `globstar` para obtener este comportamiento.

---

## 7. Archivos ocultos y comodines

En Linux, los archivos cuyo nombre comienza con un punto (`.`) son considerados ocultos.

Ejemplos:

- `.bashrc`
    
- `.profile`
    
- `.config`
    
- `.ssh`
    

Por defecto, en Bash el patrón `*` no incluye estos archivos ocultos.

```bash
# No incluye normalmente archivos ocultos
ls *

# Incluye archivos ocultos, excepto . y ..
ls .*
```

Para mostrar archivos normales y ocultos sin depender de dos patrones:

```bash
ls -a
```

> [!IMPORTANT]  
> No confundas los archivos ocultos con archivos protegidos. El punto inicial solamente hace que no aparezcan en listados normales sin la opción adecuada.

---

## 8. Diferencia entre comodines y expresiones regulares

Aunque pueden parecer similares, los comodines (_globbing_) y las expresiones regulares (_regex_) son mecanismos diferentes.

|Comodines (Glob)|Expresiones regulares (Regex)|
|---|---|
|`*`|`.*`|
|`?`|`.`|
|`[0-9]`|`[0-9]`|
|Se utilizan para seleccionar nombres de archivos.|Se utilizan para buscar patrones dentro de textos.|

Por ejemplo:

```bash
# Comodín: seleccionar archivos .txt
ls *.txt

# Expresión regular: buscar líneas que contengan números
grep '[0-9]' archivo.txt
```

**Recuerda:** en regex, `*` no significa cualquier cantidad de caracteres por sí solo. Significa repetir cero o más veces el elemento anterior.

---

## 9. Evitar que Bash interprete un comodín

En ocasiones queremos utilizar un asterisco como texto literal, sin que Bash lo expanda.

Para eso podemos utilizar comillas o una barra invertida `\`.

```bash
# Imprimir el carácter *
echo "*"

# También podemos escapar el carácter
echo \*
```

Esto resulta útil cuando queremos pasar patrones a otros comandos sin que el shell los interprete antes.

---

## 10. Precauciones de seguridad

Los comodines pueden ser muy útiles, pero también peligrosos cuando se utilizan con comandos que modifican o eliminan archivos.

Por ejemplo:

```bash
rm *.txt
```

Elimina todos los archivos `.txt` del directorio actual.

Antes de utilizar `rm`, es recomendable comprobar qué archivos coinciden con el patrón:

```bash
# Primero comprobar los archivos seleccionados
printf '%s\n' *.txt

# Después, si estás seguro, eliminarlos
rm -- *.txt
```

> [!DANGER] Atención  
> Nunca ejecutes comandos destructivos con comodines sin comprobar primero qué archivos van a ser seleccionados.

---

## 11. Resumen rápido (Cheatsheet)

|Comodín|Función|
|---|---|
|`*`|Cualquier cantidad de caracteres|
|`?`|Exactamente un carácter|
|`[abc]`|Un carácter entre los especificados|
|`[0-9]`|Un carácter dentro del rango|
|`[!abc]`|Un carácter que no esté entre los especificados|
|`{a,b,c}`|Genera varias cadenas (expansión de llaves)|
|`{1..5}`|Genera una secuencia|
|`**`|Coincidencias recursivas en Bash con `globstar` activado|

---

## Ver también

- [[MOC-programacion]]
- [[Ruta Bash]]
- [[Comandos de Linux]]
- [[01-Introducción a Bash]]
- [[02-Variables y tipos de datos en Bash]]
