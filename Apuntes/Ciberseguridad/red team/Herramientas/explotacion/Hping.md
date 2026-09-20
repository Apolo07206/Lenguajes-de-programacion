---
title: Hping
area: ciberseguridad
tipo: herramienta
tags: [ciberseguridad, red-team, explotacion]
created: 2026-01-18
updated: 2026-09-20
related: ["[[MOC-ciberseguridad]]", "[[red-team-killchain]]", "[[Nmap]]", "[[Metasploit]]"]
---

Herramienta para crear y enviar paquetes TCP, UDP e ICMP personalizados. Sirve para:

- Reconocimiento de hosts
    
- Fingerprinting de sistemas
    
- Medición de latencia
    
- Traceroute a bajo nivel
    
- Pruebas de firewall
    
- Detección de reglas de filtrado
    
- Simulación de ataques DoS
    

Es básicamente un "fabricador de paquetes" que te deja manipular flags y campos a nivel TCP/IP.


https://youtu.be/1lwr716kX30?si=wgGOxI4U5SYctyUL

---

## Usos prácticos

### **1. Comprobar si un host está vivo**

Modo ping (ICMP):

`hping3 -1 <ip>`

(`-1` = ICMP)

### **2. Syn scan rápido**

Envía SYN para ver puertos abiertos:

`hping3 -S <ip> -p <puerto>`

Si responde `SYN/ACK` => abierto  
Si responde `RST` => cerrado  
Si no responde => filtrado

### **3. Escanear rango de puertos**

`hping3 -S <ip> -p ++1`

(`++1` incrementa puertos)

### **4. Conectar por TCP (como netcat)**

`hping3 <ip> -p <puerto> -S -c 1`

(`-c 1` solo un paquete)

### **5. UDP scan**

`hping3 --udp -p <puerto> <ip>`

---

## Detección de firewalls y reglas

### **Ver si un firewall bloquea ICMP**

`hping3 -1 <ip>`

No respuestas = posible filtrado ICMP

### **Probar filtrado TCP**

Enviar SYN:

`hping3 -S <ip> -p 80`

Respuestas típicas:

- `SYN/ACK` => permitido
    
- `RST` => puerto cerrado pero no filtrado
    
- ningún paquete => filtrado
    

### **Comprobar TTL y tiempos**

`hping3 -S <ip> -p 80 -t <ttl>`

Permite identificar hop, salto o sistema filtrando.

---

## Traceroute a bajo nivel

Hping puede hacer traceroute TCP (útil cuando ICMP está bloqueado):

`hping3 -S <ip> -p 443 --traceroute`

Si la red bloqueó `traceroute` normal, este a veces pasa.

---

## Fingerprinting (detección de OS)

Hping muestra valores útiles como:

- TTL
    
- Ventana TCP
    
- Flags
    
- Size
    

Comparando TTL+Window puedes inferir sistema:

- TTL 64 => Linux/macOS
    
- TTL 128 => Windows
    
- TTL 255 => Routers o BSD
    

Ejemplo:

`hping3 -S <ip> -p 80 -c 1`

---

## Simulación de ataques DoS _(laboratorio)_

### **Syn flood**

`hping3 -S <ip> -p 80 --flood`

### **UDP flood**

`hping3 --udp <ip> --flood`

**Nota táctica:** Esto solo en laboratorios, nunca en redes reales.

---

## Flags comunes en TCP

|Flag|Significado|
|---|---|
|`-S`|SYN|
|`-A`|ACK|
|`-R`|RST|
|`-F`|FIN|
|`-P`|PUSH|
|`-U`|URG|

Puedes combinarlas para comportamiento raro y ver cómo responde una máquina.

---

## Cosas que Hping te deja manipular

Campos del paquete:

- TTL
    
- Flags
    
- Puerto origen y destino
    
- Spoof de IP
    
- Tiempos
    
- Tamaño del paquete
    
- Protocolo (TCP/UDP/ICMP)
    

Ejemplo spoof IP:

`hping3 -S -a 1.2.3.4 <victima> -p 80`

---

## Cuándo usar Hping en pentesting

Cuando quieres:

- Descubrir hosts sin ICMP
    
- Analizar firewalls
    
- Evadir IDS básico
    
- Hacer reconocimiento silencioso
    
- Saber qué puertos realmente pasan
    
- Saber qué protocolo permite la red
    
- Probar DDoS en laboratorio
    
- Obtener fingerprint de SO
    

---

## Relación con Ferney (útil para ti)

Como tú ya usas Nmap y Metasploit, Hping te da:

- **más control** que Nmap
    
- **menos automatización**, más "paquete artesanal"
    
- mejor análisis de **filtrado firewall**
    
- útiles datos para **bypasses**
    
- base para entender **Packet Crafting** (Scapy)
    

Además conecta bien con tu interés en Red Team.

---

## Frase del pingüino

“Hping es como hablar TCP sin traductor. Si le pones flags, te responde con flags.” 🐧📡

## Ver también

- [[MOC-ciberseguridad]]
- [[red-team-killchain]]
- [[Nmap]]
- [[Metasploit]]
