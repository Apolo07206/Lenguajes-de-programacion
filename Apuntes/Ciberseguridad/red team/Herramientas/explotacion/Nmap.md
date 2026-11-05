---
title: Nmap
area: ciberseguridad
tipo: herramienta
tags: [ciberseguridad, red-team, explotacion]
created: 2026-01-18
updated: 2026-10-12
related: ["[[MOC-ciberseguridad]]", "[[red-team-killchain]]", "[[Metasploit]]", "[[Wireshark]]", "[[Searchsploit]]", "[[redes y protocolos]]", "[[Sockets]]", "[[apuntes normales]]"]
---

## ¿Qué es Nmap?

Herramienta para escanear redes y descubrir hosts, puertos abiertos, servicios y vulnerabilidades.

https://youtu.be/xrPtMRE3YME?si=UAhXZcCQpruTSTBx
---

## Argumentos Esenciales

### Tipos de Escaneo

**`-sS`** - TCP SYN Scan (el más usado)

- Escaneo "sigiloso", no completa la conexión
- Requiere privilegios root
- Rápido y menos detectable

bash

```bash
sudo nmap -sS 192.168.1.1
```

**`-sT`** - TCP Connect Scan

- Cuando no tienes privilegios root
- Más lento pero funciona sin sudo

bash

```bash
nmap -sT 192.168.1.1
```

**`-sU`** - UDP Scan

- Para servicios UDP (DNS, DHCP, SNMP)
- Mucho más lento que TCP

bash

```bash
sudo nmap -sU 192.168.1.1
```

**`-sV`** - Detección de versiones

- Identifica versión del servicio
- Esencial para encontrar vulnerabilidades

bash

```bash
nmap -sV 192.168.1.1
```

**`-O`** - Detección de Sistema Operativo

- Intenta identificar el OS
- Requiere root

bash

```bash
sudo nmap -O 192.168.1.1
```

### Descubrimiento de Hosts

**`-sn`** - Ping Scan (no escanea puertos)

- Solo descubre qué hosts están activos
- Rápido para mapear una red

bash

```bash
nmap -sn 192.168.1.0/24
```

**`-Pn`** - No hacer ping

- Asume que el host está activo
- Útil cuando firewall bloquea ICMP

bash

```bash
nmap -Pn 192.168.1.1
```

### Especificación de Puertos

**`-p <puertos>`** - Especificar puertos

bash

```bash
nmap -p 80,443 192.168.1.1              # Puertos específicos
nmap -p 1-1000 192.168.1.1              # Rango
nmap -p- 192.168.1.1                    # Todos los puertos (1-65535)
nmap -p U:53,T:80 192.168.1.1           # UDP y TCP específicos
```

**`-F`** - Fast scan (100 puertos más comunes)

bash

```bash
nmap -F 192.168.1.1
```

**`--top-ports <n>`** - N puertos más comunes

bash

```bash
nmap --top-ports 20 192.168.1.1
```

### Scripts NSE

**`-sC`** - Scripts por defecto

- Ejecuta scripts seguros y útiles
- Equivale a `--script=default`

bash

```bash
nmap -sC 192.168.1.1
```

**`--script <nombre>`** - Script específico

bash

```bash
nmap --script http-title 192.168.1.1
nmap --script vuln 192.168.1.1          # Scripts de vulnerabilidades
nmap --script "smb-*" 192.168.1.1       # Todos los scripts SMB
```

### Timing y Performance

**`-T<0-5>`** - Templates de velocidad

- `-T0`: Paranoico (muy lento)
- `-T1`: Sneaky
- `-T2`: Polite
- `-T3`: Normal (predeterminado)
- `-T4`: Aggressive (recomendado)
- `-T5`: Insane (muy rápido, puede perder precisión)

bash

```bash
nmap -T4 192.168.1.1
```

### Output

**`-oN <archivo>`** - Output normal (texto)

bash

```bash
nmap -oN scan.txt 192.168.1.1
```

**`-oX <archivo>`** - Output XML

bash

```bash
nmap -oX scan.xml 192.168.1.1
```

**`-oA <basename>`** - Todos los formatos

bash

```bash
nmap -oA scan_results 192.168.1.1
# Crea: scan_results.nmap, scan_results.xml, scan_results.gnmap
```

**`-v`** - Verbose (más información)

bash

```bash
nmap -v 192.168.1.1
nmap -vv 192.168.1.1  # Aún más detallado
```

### Opciones Útiles

**`-A`** - Aggressive scan

- Combina: `-O -sV -sC --traceroute`
- Detección completa pero ruidosa

bash

```bash
nmap -A 192.168.1.1
```

**`--open`** - Solo mostrar puertos abiertos

bash

```bash
nmap --open 192.168.1.0/24
```

**`-n`** - No resolver DNS

- Más rápido

bash

```bash
nmap -n 192.168.1.1
```

---

## Scripts NSE Más Usados

### Categorías de Scripts

bash

```bash
--script default       # Scripts seguros y útiles (predeterminado con -sC)
--script vuln          # Detectar vulnerabilidades
--script safe          # Solo scripts seguros
--script auth          # Scripts de autenticación
--script brute         # Fuerza bruta
--script discovery     # Descubrimiento de información
```

### Scripts HTTP

**Información básica:**

bash

```bash
nmap --script http-title -p 80 192.168.1.1
nmap --script http-headers -p 80 192.168.1.1
nmap --script http-methods -p 80 192.168.1.1
nmap --script http-enum -p 80 192.168.1.1           # Enumerar directorios
```

**Vulnerabilidades web:**

bash

```bash
nmap --script http-vuln-* -p 80,443 192.168.1.1     # Todas las vulns HTTP
nmap --script http-sql-injection -p 80 192.168.1.1
nmap --script http-shellshock -p 80 192.168.1.1
nmap --script http-csrf -p 80 192.168.1.1
```

**WAF Detection:**

bash

```bash
nmap --script http-waf-detect -p 80,443 192.168.1.1
nmap --script http-waf-fingerprint -p 80,443 192.168.1.1
```

### Scripts SMB (Windows)

**Información SMB:**

bash

```bash
nmap --script smb-os-discovery -p 445 192.168.1.1
nmap --script smb-protocols -p 445 192.168.1.1
nmap --script smb-security-mode -p 445 192.168.1.1
```

**Enumeración SMB:**

bash

```bash
nmap --script smb-enum-shares -p 445 192.168.1.1
nmap --script smb-enum-users -p 445 192.168.1.1
nmap --script smb-enum-domains -p 445 192.168.1.1
nmap --script smb-enum-sessions -p 445 192.168.1.1
```

**Vulnerabilidades SMB (críticas):**

bash

```bash
nmap --script smb-vuln-ms17-010 -p 445 192.168.1.1  # EternalBlue
nmap --script smb-vuln-ms08-067 -p 445 192.168.1.1
nmap --script smb-vuln-* -p 445 192.168.1.1         # Todas las vulns SMB
```

### Scripts SSH

bash

```bash
nmap --script ssh-auth-methods -p 22 192.168.1.1
nmap --script ssh-hostkey -p 22 192.168.1.1
nmap --script ssh-brute --script-args userdb=users.txt,passdb=pass.txt -p 22 192.168.1.1
```

### Scripts FTP

bash

```bash
nmap --script ftp-anon -p 21 192.168.1.1            # FTP anónimo
nmap --script ftp-bounce -p 21 192.168.1.1
nmap --script ftp-syst -p 21 192.168.1.1
nmap --script ftp-brute -p 21 192.168.1.1
```

### Scripts DNS

bash

```bash
nmap --script dns-brute example.com
nmap --script dns-zone-transfer --script-args dns-zone-transfer.domain=example.com -p 53 192.168.1.1
nmap --script dns-recursion -p 53 192.168.1.1
```

### Scripts MySQL

bash

```bash
nmap --script mysql-info -p 3306 192.168.1.1
nmap --script mysql-databases -p 3306 192.168.1.1
nmap --script mysql-users -p 3306 192.168.1.1
nmap --script mysql-empty-password -p 3306 192.168.1.1
nmap --script mysql-brute -p 3306 192.168.1.1
```

### Scripts MongoDB

bash

```bash
nmap --script mongodb-info -p 27017 192.168.1.1
nmap --script mongodb-databases -p 27017 192.168.1.1
```

### Scripts RDP

bash

```bash
nmap --script rdp-enum-encryption -p 3389 192.168.1.1
nmap --script rdp-vuln-ms12-020 -p 3389 192.168.1.1
```

### Scripts SSL/TLS

bash

```bash
nmap --script ssl-cert -p 443 192.168.1.1
nmap --script ssl-enum-ciphers -p 443 192.168.1.1
nmap --script ssl-heartbleed -p 443 192.168.1.1
nmap --script ssl-poodle -p 443 192.168.1.1
```

### Scripts de Vulnerabilidades Generales

bash

```bash
nmap --script vuln -p- 192.168.1.1                  # Todas las vulnerabilidades
nmap --script exploit -p- 192.168.1.1               # Scripts de explotación
```

---

## Ejemplos Prácticos Comunes

### Reconocimiento Básico

**Descubrir hosts activos en la red:**

bash

```bash
nmap -sn 192.168.1.0/24
```

**Escaneo rápido de puertos comunes:**

bash

```bash
nmap -F 192.168.1.1
```

**Escaneo completo de un host:**

bash

```bash
nmap -p- -sV -sC -T4 -oA full_scan 192.168.1.1
```

### Escaneos de Servicios Específicos

**Buscar servidores web:**

bash

```bash
nmap -p 80,443,8080,8443 --open 192.168.1.0/24
```

**Buscar servicios de base de datos:**

bash

```bash
nmap -p 3306,5432,1433,27017,6379 --open 192.168.1.0/24
```

**Buscar servicios de administración remota:**

bash

```bash
nmap -p 22,23,3389,5900 --open 192.168.1.0/24
```

### Escaneo con Detección de Versiones

**Escaneo estándar con versiones:**

bash

```bash
nmap -sV -T4 -p- 192.168.1.1
```

**Aggressive scan (todo incluido):**

bash

```bash
nmap -A -T4 192.168.1.1
```

### Búsqueda de Vulnerabilidades

**Escaneo completo de vulnerabilidades:**

bash

```bash
nmap --script vuln -sV -p- 192.168.1.1
```

**Buscar EternalBlue en red Windows:**

bash

```bash
nmap -p 445 --script smb-vuln-ms17-010 192.168.1.0/24
```

**Vulnerabilidades web:**

bash

```bash
nmap -p 80,443 --script http-vuln-* example.com
```

### Escaneos UDP

**Escaneo UDP de servicios comunes:**

bash

```bash
sudo nmap -sU --top-ports 20 192.168.1.1
```

**Combinar TCP y UDP:**

bash

```bash
sudo nmap -sS -sU -p T:80,443,T:22,U:53,161,500 192.168.1.1
```

### Con Output

**Guardar resultados completos:**

bash

```bash
nmap -A -p- -T4 -oA scan_completo 192.168.1.1
```

**Escaneo rápido con verbose:**

bash

```bash
nmap -sV -T4 -v -oN quick_scan.txt 192.168.1.0/24
```

---

## Workflows Comunes de Pentesting

### Workflow 1: Reconocimiento Rápido

bash

```bash
# 1. Descubrir hosts activos
nmap -sn 192.168.1.0/24 -oA hosts

# 2. Escaneo rápido de puertos
nmap -iL hosts.txt -F -oA quick_ports

# 3. Escaneo detallado de hosts con puertos abiertos
nmap -sV -sC -p- -T4 -oA detailed <IPs_con_puertos>
```

### Workflow 2: Escaneo Completo

bash

```bash
# 1. Todos los puertos TCP
nmap -p- -T4 192.168.1.1 -oA all_tcp

# 2. Versiones y scripts en puertos abiertos
nmap -sV -sC -p <puertos_abiertos> 192.168.1.1 -oA services

# 3. Vulnerabilidades
nmap --script vuln -p <puertos_abiertos> 192.168.1.1 -oA vulns
```

### Workflow 3: Escaneo de Red Empresarial

bash

```bash
# 1. Ping sweep sin escaneo de puertos
nmap -sn -T4 10.0.0.0/8 -oA network_discovery

# 2. Top 1000 puertos en hosts activos
nmap -iL live_hosts.txt --top-ports 1000 -T4 -oA top_ports

# 3. Servicios críticos
nmap -iL live_hosts.txt -sV -p 21,22,23,80,443,445,3389 -oA critical_services
```

---

## Combinaciones Más Usadas

### El Clásico

bash

```bash
nmap -sV -sC -p- -T4 -oA scan 192.168.1.1
```

- `-sV`: Detecta versiones
- `-sC`: Scripts por defecto
- `-p-`: Todos los puertos
- `-T4`: Rápido
- `-oA`: Guarda en todos los formatos

### El Agresivo

bash

```bash
nmap -A -T4 -p- -oA aggressive_scan 192.168.1.1
```

- `-A`: OS detection + versión + scripts + traceroute
- Todo en uno, muy completo

### El Sigiloso

bash

```bash
nmap -sS -T2 -f --data-length 50 192.168.1.1
```

- `-sS`: SYN scan
- `-T2`: Más lento (menos detectable)
- `-f`: Fragmentar paquetes

### Búsqueda de Vulnerabilidades

bash

```bash
nmap --script vuln -sV -p- -T4 192.168.1.1
```

### Quick Win

bash

```bash
nmap --script "default or vuln" -sV --top-ports 100 -T4 192.168.1.1
```

---

## Tips Prácticos

### Comandos Útiles

**Ver ayuda de un script:**

bash

```bash
nmap --script-help http-vuln-*
```

**Actualizar base de datos de scripts:**

bash

```bash
sudo nmap --script-updatedb
```

**Listar scripts disponibles:**

bash

```bash
ls /usr/share/nmap/scripts/ | grep http
```

### Para Acelerar Escaneos

bash

```bash
# Usar -T4
nmap -T4 192.168.1.1

# No resolver DNS
nmap -n 192.168.1.1

# Solo puertos abiertos
nmap --open 192.168.1.0/24

# Top ports en lugar de todos
nmap --top-ports 100 192.168.1.1
```

### Para Mayor Precisión

bash

```bash
# Más intentos
nmap --max-retries 5 192.168.1.1

# Mayor intensidad en detección de versión
nmap -sV --version-intensity 9 192.168.1.1

# Timing más lento pero preciso
nmap -T2 192.168.1.1
```

---

## Estados de Puerto

**`open`** - Puerto abierto, servicio aceptando conexiones **`closed`** - Puerto accesible pero no hay servicio **`filtered`** - Firewall bloqueando, no se puede determinar **`open|filtered`** - Nmap no puede determinar (común en UDP) **`unfiltered`** - Accesible pero estado desconocido (solo con -sA)

---

## Wordlists Comunes (Kali Linux)

bash

```bash
/usr/share/wordlists/rockyou.txt
/usr/share/wordlists/dirb/common.txt
/usr/share/seclists/Discovery/DNS/subdomains-top1million.txt
```

## Ver también

- [[MOC-ciberseguridad]]
- [[red-team-killchain]]
- [[Metasploit]]
- [[Wireshark]]
- [[Searchsploit]]
- [[redes y protocolos]]
- [[Sockets]]
- [[apuntes normales]]
