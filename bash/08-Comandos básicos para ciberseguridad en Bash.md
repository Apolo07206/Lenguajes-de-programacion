---
aliases: ["Comandos básicos para ciberseguridad en Bash", "Comandos de ciberseguridad en Bash", "Comandos de Linux"]
title: Comandos básicos para ciberseguridad en Bash
area: programacion
tipo: herramienta
tags: [programacion, bash, ciberseguridad]
created: 2026-10-25
updated: 2026-10-25
related: ["[[MOC-programacion]]", "[[Ruta Bash]]", "[[MOC-ciberseguridad]]", "[[Permisos, Servicios y Logs en Linux]]", "[[Comandos de Linux]]"]
---

# Comandos básicos para ciberseguridad en Bash

Cada apartado dice **para qué sirve el comando** y **cómo se usa** con un ejemplo real.

> Lista completa, sin nada fuera → [[Comandos de Linux]] (referencia por áreas: paquetes, discos, logs, kernel, anonimato, wifi…).

---

## 1. Navegación — ¿dónde estoy y qué hay aquí?

Sirve para moverte por el sistema de archivos y ver su contenido. Es lo primero que haces al entrar en una máquina.

```bash
pwd
cd /var/log
cd
cd ..
cd -
```

|Comando|Para qué sirve|
|---|---|
|`pwd`|muestra la ruta absoluta del directorio actual|
|`cd /ruta`|se mueve a una ruta absoluta|
|`cd`|vuelve a tu home (`~`)|
|`cd ..`|sube un nivel|
|`cd -`|vuelve al directorio anterior|
|`cd ~/docs`|va a una carpeta de tu home|
|`cd ../otra`|sube un nivel y entra en otra carpeta|

```bash
ls -lah
ls -lR /etc
ls -lt
ls -lS
```

|Comando|Para qué sirve|
|---|---|
|`ls -la`|lista largo con archivos ocultos|
|`ls -lah`|lo mismo, con tamaños legibles (K, M, G)|
|`ls -lR /etc`|recursivo: baja por todas las subcarpetas|
|`ls -lt`|ordenado por fecha, lo último arriba|
|`ls -lS`|ordenado por tamaño, lo más grande arriba|
|`tree -L 2`|árbol visual de 2 niveles|

Cómo se lee una línea de `ls -l`:

```
-rwxr-xr-x 1 ferxxo ferxxo 42 oct 25 12:00 script.sh
│          │    │      │    │      │        │
tipo       enl. dueño  grupo tamaño fecha   nombre
```

El primer carácter es el tipo: `-` archivo, `d` directorio, `l` enlace simbólico. Los 9 siguientes son permisos (ver §5).

---

## 2. Hardware y recursos — ¿qué máquina tengo delante?

Sirve para sacar la ficha técnica del equipo: CPU, memoria, discos. Es el reconocimiento previo antes de cualquier cosa.

```bash
lscpu
free -h
lsblk
df -h
uname -a
du -sh  Descargas/
```

|Comando|Para qué sirve|Ejemplo de uso|
|---|---|---|
|`lscpu`|modelo de CPU, núcleos, arquitectura, si soporta virtualización|`lscpu \| grep -i virtual`|
|`free -h`|RAM y swap en formato legible|`free -h \| grep Mem`|
|`lsblk`|discos, particiones y puntos de montaje|`lsblk -f` añade filesystem y UUID|
|`df -h`|espacio **libre** por partición|`df -hT` incluye el tipo de filesystem|
|`du -sh /var`|cuánto pesa un directorio|`du -sh * \| sort -rh \| head` = lo más pesado|
|`uname -a`|kernel, arquitectura y SO|`uname -r` solo la versión del kernel|
|`hostnamectl`|nombre, SO exacto, kernel, arquitectura|`hostnamectl \| grep Operating`|
|`uptime`|tiempo encendido y carga media|`uptime`|
|`lspci` / `lsusb`|tarjetas PCI / dispositivos USB|`lsusb -v \| less`|

> El significado de cada componente está en [[Arquitectura Básica de Hardware]].

---

## 3. Lectura de archivos — ver el contenido sin romper nada

Sirve para inspeccionar archivos de configuración, logs y binarios. En ciberseguridad se usa para leer `/etc`, logs de autenticación y para mirar dentro de un ejecutable.

```bash
cat /etc/passwd
head -20 auth.log
tail -20 auth.log
tail -f /var/log/syslog
wc -l auth.log
```

|Comando|Para qué sirve|
|---|---|
|`cat archivo`|muestra el archivo entero|
|`less archivo`|lo muestra paginado, se sale con `q`|
|`head -20 archivo`|primeras 20 líneas|
|`tail -20 archivo`|últimas 20 líneas|
|`tail -f archivo`|lo sigue **en vivo**; `Ctrl`+`C` para parar|
|`wc -l archivo`|cuántas líneas tiene|

```bash
file /bin/ls
head -1 /usr/bin/apt
strings malware.bin | head -40
```

|Comando|Para qué sirve y cómo se usa|
|---|---|
|`file <ruta>`|dice qué es realmente un archivo: binario ELF, script, imagen, ZIP… Se usa para saber si un archivo sin extensión es ejecutable.|
|`head -1 <ruta>`|la primera línea de un script es el shebang (`#!/bin/bash`) y revela qué intérprete necesita.|
|`strings <binario>`|extrae las cadenas legibles de un binario: URLs, rutas, mensajes de error, contraseñas hardcodeadas. `strings f \| grep -i pass`.|

Distinguir binario de script → [[Concepto de binarios y scripts]].

---

## 4. Búsqueda

### `find` — localizar archivos en el árbol

**Para qué sirve:** buscar archivos y directorios por cualquier criterio: nombre, permisos, tamaño, fecha, dueño. Es la herramienta de reconocimiento de filesystem por excelencia.

Cómo se arma:

```bash
find /var/www -name "*.php" -size +1M -mtime -7
       │            │            │         │
     dónde       qué nombre   qué peso   qué fecha
```

Ejemplos reales:

```bash
find / -name "*.conf"
find . -iname "readme*"
find /etc -type f
find / -perm -4000 2>/dev/null
find / -writable -type d 2>/dev/null
find . -type f -exec ls -lh {} \;
```

|Parte del comando|Para qué sirve|
|---|---|
|`-name "*.conf"`|por nombre; admite `*` y `?`|
|`-iname "readme*"`|igual pero sin distinguir mayúsculas|
|`-type f` / `-type d`|solo archivos / solo directorios|
|`-perm -4000`|binarios con **SUID** → posible escalada de privilegios|
|`-perm -2000`|binarios con **SGID**|
|`-writable -type d`|directorios donde puedes escribir (vector de ataque)|
|`-size +100M`|archivos de más de 100 MB|
|`-mtime -1`|modificados en el último día|
|`-user root`|dueño root|
|`-maxdepth 2`|no baje de 2 niveles (más rápido)|
|`-exec ls -lh {} \;`|ejecuta `ls -lh` en cada resultado|

`2>/dev/null` al final escribe en `/dev/null` los errores de "Permiso denegado" y deja la salida limpia.

### `grep` — buscar dentro del contenido

**Para qué sirve:** buscar texto **dentro** de los archivos. Es el comando más usado en ciber para rastrear contraseñas, IPs, usuarios o errores en logs.

```bash
grep "Failed password" /var/log/auth.log
grep -r "password" /etc
grep -rn "TODO" .
grep -i "error" log.txt
ps aux | grep sshd
```

|Flag|Para qué sirve|
|---|---|
|`-r`|recursivo: baja por todos los subdirectorios|
|`-n`|muestra el número de línea|
|`-i`|ignora mayúsculas y minúsculas|
|`-v`|invierte: muestra las líneas que **NO** coinciden|
|`-c`|solo cuenta cuántas coincidencias hay|
|`-w`|coincide solo con la palabra completa|
|`-E`|expresión regular extendida|
|`-l`|solo imprime el **nombre** de los archivos con coincidencia|
|`-A 3 -B 3`|3 líneas antes / después del hallazgo|
|`\|`|encadena: el resultado de un comando entra en otro|

---

## 5. Permisos y propiedad

**Para qué sirve:** ver y cambiar quién puede leer, escribir y ejecutar cada archivo. En ciber, la primera pregunta de una escalada es "¿qué puedo modificar?".

```bash
ls -l script.sh
chmod 755 script.sh
chmod +x script.sh
chmod 600 id_rsa
chown www-data:www-data /var/www
```

Cómo se calcula el número:

```
755  →  dueño rwx (4+2+1) · grupo r-x (4+1) · otros r-x (4+1)
600  →  dueño rw- · grupo --- · otros ---   (solo tú lo tocas)
```

|Número|Permisos|Significado|
|---|---|---|
|`7`|`rwx`|leer + escribir + ejecutar|
|`6`|`rw-`|leer + escribir|
|`5`|`r-x`|leer + ejecutar|
|`4`|`r--`|solo leer|
|`0`|`---`|nada|

|Comando|Para qué sirve|
|---|---|
|`chmod +x f`|hace ejecutable un script (obligatorio antes de `./script.sh`)|
|`chmod 600 id_rsa`|clave SSH legible solo por su dueño|
|`chmod -R 755 dir`|aplica el permiso a todo el árbol|
|`chown usuario:grupo f`|cambia dueño y grupo|
|`lsattr f`|atributos del sistema; con `i` el archivo no se puede borrar ni modificar|

Detallado en [[Permisos, Servicios y Logs en Linux]].

---

## 6. Procesos — qué está corriendo y cómo pararlo

**Para qué sirve:** ver qué procesos hay, cuánto consumen y terminar los que no debían estar ahí (o los tuyos).

```bash
ps aux
ps aux | sort -k3 -rn | head
top
kill -9 1234
pgrep -l sshd
```

|Comando|Para qué sirve y cómo se usa|
|---|---|
|`ps aux`|lista **todos** los procesos con usuario, PID, %CPU y %MEM. Es el comando base: `ps aux \| grep <nombre>` para localizar un proceso y copiar su PID.|
|`ps -ef`|lo mismo en formato System V (más compacto)|
|`top`|vista en tiempo real; se sale con `q`|
|`htop`|igual pero coloreado y con barras|
|`kill PID`|termina el proceso con la señal 15 (SIGTERM): le pide que se cierre ordenadamente|
|`kill -9 PID`|señal 9 (SIGKILL): lo mata a la fuerza, no se puede ignorar|
|`kill -STOP PID`|pausa el proceso (señal 19)|
|`killall nombre`|mata por el nombre del proceso|
|`pkill -f "python srv"`|mata por coincidencia en la línea de comando completa|
|`pgrep -l sshd`|devuelve el PID y el nombre sin tener que hacer `ps \| grep`|
|`jobs` / `bg` / `fg`|trabajos de la terminal actual: en segundo plano, reanudar, traer al frente|
|`nohup cmd &`|que siga corriendo aunque cierres la terminal|

Flujo típico: `ps aux | grep proceso` → copiar el PID → `kill -9 PID`.

---

## 7. Servicios: systemd y systemctl

**Para qué sirve:** arrancar, parar y configurar los servicios del sistema (SSH, apache, mysql…) y revisar sus logs. `systemd` es el gestor de arranque de Linux moderno y `systemctl` su interfaz.

```bash
systemctl status sshd
systemctl restart sshd
systemctl enable sshd
journalctl -u sshd -f
```

|Comando|Para qué sirve|
|---|---|
|`systemctl status <s>`|estado del servicio: si corre, desde cuándo, últimos errores|
|`systemctl start <s>`|lo arranca ahora mismo|
|`systemctl stop <s>`|lo para|
|`systemctl restart <s>`|lo reinicia — se usa tras cambiar su configuración|
|`systemctl reload <s>`|recarga la config **sin cortar** las conexiones activas|
|`systemctl enable <s>`|lo pone en autoarranque al encender|
|`systemctl disable <s>`|quita el autoarranque|
|`systemctl is-enabled <s>`|¿arranca con el equipo?|
|`systemctl is-active <s>`|¿está corriendo ahora?|

```bash
systemctl list-units --type=service
systemctl list-unit-files | grep enabled
systemctl list-units --failed
systemctl daemon-reload
systemd-analyze blame
```

|Comando|Para qué sirve|
|---|---|
|`list-units --type=service`|todos los servicios registrados|
|`list-unit-files \| grep enabled`|los que arrancan solos — útil para ver persistencia sospechosa|
|`list-units --failed`|servicios caídos, primer diagnóstico de un fallo|
|`daemon-reload`|obligatorio tras editar un archivo `.service` a mano|
|`systemd-analyze blame`|qué unidad retrasó el arranque y cuánto|

Variantes legacy de initV que aún aparecen en scripts antiguos: `service ssh status`, `service ssh restart`.

Logs de un servicio con `journalctl`:

|Comando|Para qué sirve|
|---|---|
|`journalctl -u sshd -f`|logs de sshd en vivo|
|`journalctl -u sshd --since "1 hour ago"`|solo la última hora|
|`journalctl -b`|desde este arranque|
|`journalctl -p err`|solo errores|
|`journalctl -xe`|errores + últimas líneas, para ver qué acaba de pasar|

Mapa completo en [[Permisos, Servicios y Logs en Linux]].

---

## 8. Red — qué se escucha, con quién hablo

**Para qué sirve:** ver interfaces, puertos abiertos y conexiones; hacer peticiones y levantar un listener.

```bash
ip a
ss -tulnp
curl http://target.com
nc -lvnp 4444
```

|Comando|Para qué sirve y cómo se usa|
|---|---|
|`ip a`|interfaces y IPs de la máquina. Reemplaza al viejo `ifconfig`.|
|`ip r`|tabla de rutas: por dónde sale el tráfico|
|`ip neigh`|vecinos de red (caché ARP)|
|`ss -tulnp`|**puertos abiertos**: `-t` TCP, `-u` UDP, `-l` escuchando, `-n` sin resolver nombres, `-p` qué proceso los abre. Reemplaza a `netstat`.|
|`hostname -I`|solo las IPs de esta máquina|
|`curl <url>`|petición HTTP. `curl -I` solo las cabeceras, `curl -s url \| head` en silencio|
|`wget <archivo>`|descarga un archivo|
|`nc -lvnp 4444`|levanta un **listener** en el puerto 4444: `-l` escuchar, `-v` verbose, `-n` sin DNS, `-p` puerto. Base de las reverse shells.|
|`nc -zv ip 1-1024`|sondeo de puertos: `-z` solo escanea, no manda datos|
|`ping -c 4 8.8.8.8`|4 paquetes y para (`-c` = count)|
|`traceroute target`|ruta que sigue el paquete hasta el destino|

---

## 9. Identidad, privilegios y sesiones

**Para qué sirve:** saber quién eres, qué puedes hacer y quién más está dentro. Primer paso de toda enumeración.

```bash
whoami
id
sudo -l
last
history
```

|Comando|Para qué sirve|
|---|---|
|`whoami`|usuario con el que estás trabajando|
|`id`|UID, GID y todos tus grupos — de ahí sale si eres `sudo`, `docker`, `disk`…|
|`sudo -l`|**qué puedes ejecutar como root sin contraseña**. Punto de partida de cualquier privesc|
|`sudo -i`|te da una shell completa de root|
|`last`|quién ha entrado antes y cuándo|
|`lastb`|intentos de login fallidos — ahí se ve un brute force|
|`w` / `who`|quién está conectado ahora mismo|
|`history`|comandos que se han ejecutado en esta shell|
|`which cmd`|qué binario se ejecuta al escribir ese comando|
|`type cmd`|si es binario, alias o función|
|`man cmd`|manual del comando (`q` para salir)|

---

## 10. Archivos: copiar, mover, borrar, comprimir

```bash
cp -r origen destino
mv viejo nuevo
rm -rf directorio
tar -czvf backup.tar.gz carpeta/
ln -s /ruta real enlace
```

|Comando|Para qué sirve|
|---|---|
|`touch f`|crea un archivo vacío o actualiza su fecha|
|`cp -r a b`|copia recursiva de directorios|
|`mv viejo nuevo`|mueve o renombra|
|`rm -rf dir`|borra sin preguntar y todo lo de dentro — irreversible|
|`ln -s /ruta enlace`|enlace simbólico: un atajo que apunta a otra ruta|
|`tar -czvf x.tar.gz dir/`|comprime: `-c` crear, `-z` gzip, `-v` listar, `-f` nombre del archivo|
|`tar -xzvf x.tar.gz`|descomprime: `-x` extraer|
|`zip -r x.zip dir/` / `unzip x.zip`|igual en formato zip|

---

## 11. Logs del sistema

**Para qué sirve:** reconstruir qué ha pasado: quién entró, qué falló, si hay un intruso.

```bash
ls -la /var/log/
tail -f /var/log/auth.log
grep "Failed password" /var/log/auth.log | wc -l
```

|Archivo|Contenido|
|---|---|
|`/var/log/auth.log`|autenticaciones, sudo, SSH (Debian/Ubuntu)|
|`/var/log/syslog`|mensajes generales del sistema (Debian/Ubuntu)|
|`/var/log/secure`|autenticaciones (RHEL/CentOS)|
|`/var/log/kern.log`|mensajes del kernel|

El ejemplo de arriba cuenta cuántos intentos de contraseña han fallado.

---

## 12. Plantilla: ficha técnica del equipo

```bash
lscpu | head -12
free -h
lsblk
ip a
file /bin/ls
```

```markdown
# Ficha técnica de mi Equipo

## 1. Hardware y red
- **CPU:** <lscpu>
- **RAM total y libre:** <free -h>
- **Almacenamiento:** <lsblk>
- **Red:** <ip a>

## 2. Programas verificados con `file`
- Binarios ELF: /bin/ls, /bin/grep
- Scripts/intérpretes: /bin/bash, /usr/bin/python3

## 3. Virtualización
- **RAM libre para VMs:** <free -h>
```

---

## Trampas típicas

- `lsblk` no da espacio libre: eso es `df -h`.
- `kill` a veces no basta → `kill -9`.
- `systemctl restart` corta conexiones; usa `reload` si el servicio lo admite.
- `netstat` y `ifconfig` están deprecados: usa `ss` e `ip`.
- `find / -name ... 2>/dev/null` evita el ruido de "Permiso denegado".
- `rm -rf` no tiene vuelta atrás.

### Nuevas trampas 2026

- **Expansión de tilde en variables**: `~/path` puede comportarse de forma inesperada en scripts sin `export HOME`
- **Comandos ofuscados con Unicode**: Los atacantes usan caracteres Unicode invisibles para ocultar comandos maliciosos
- **Base64 en comandos de línea**: `base64 -d` usado para descodificar payloads directamente en la shell
- **Uso de `/dev/tcp` vs `/dev/udp`**: Posibles problemas de seguridad dependiendo de la configuración del sistema
- **Errores de expansión de argumentos**: `$*` vs `$@` en scripts de red pueden comportarse diferente con múltiples argumentos

## Ver también

- [[MOC-programacion]]
- [[MOC-ciberseguridad]]
- [[Ruta Bash]]
- [[Comandos de Linux]]
- [[Permisos, Servicios y Logs en Linux]]
- [[Concepto de binarios y scripts]]
- [[Arquitectura Básica de Hardware]]
- [[01-Introducción a Bash|Introducción a Bash]]
