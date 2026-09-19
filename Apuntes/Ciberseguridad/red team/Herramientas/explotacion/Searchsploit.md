Herramienta de `Exploit-DB` para buscar exploits, PoCs y referencias de vulnerabilidades desde la terminal. No descarga a lo loco; solo muestra lo que existe y te deja copiar lo necesario.

Funciona online y offline, ideal para pentesting en entornos sin internet.


https://youtu.be/yl5ny6c39_4?si=5neE1YCvP62nMgAQ
---

## Para qué sirve

- Buscar exploits según servicio, versión o CVE
    
- Encontrar PoCs para vulnerabilidades recién identificadas
    
- Ver rutas y archivos disponibles sin salir de la terminal
    
- Copiar los exploits a tu directorio local
    
- Convertirte en el bibliotecario oscuro del hacking
    

---

## Uso Básico

Buscar algo por texto:

`searchsploit <palabra>`

Ej:

`searchsploit samba`

Buscar por versión:

`searchsploit apache 2.4.49`

Buscar por CVE:

`searchsploit CVE-2017-0144`

---

## Lectura de resultados

Te muestra columnas estilo:

`Exploit Title                                     |  Path`

Ejemplo:

`Samba 3.0 - Remote Code Execution                 |  linux/remote/10XXX.c`

Con eso sabes:

1. Servicio
    
2. Versión
    
3. Tipo (local, remote, poc)
    
4. Ruta interna en tu Kali
    

---

## Actualización de la base

Para tener exploits frescos:

`searchsploit -u`

---

## Copiar Exploits

Si encuentras un exploit que quieres modificar:

`searchsploit -m <ruta_del_exploit>`

Ejemplo:

`searchsploit -m linux/remote/12345.c`

Esto lo copia al directorio actual sin corromper la base de datos.

---

## Mostrar Contenidos sin Abrir Editor

Para leerlo en consola:

`searchsploit -x <ruta_del_exploit>`

Ej:

`searchsploit -x windows/remote/1234.py`

---

## Filtrar Resultados

Mostrar solo la ruta:

`searchsploit -p <palabra>`

Mostrar resultados en color (más bonito):

`searchsploit --colour <palabra>`

---

## Función de Mirrors para CVEs

Si quieres mostrar un CVE directamente desde el mirror:

`searchsploit --cve CVE-2018-10933`

---

## Cosas que Searchsploit NO Hace

Importante para no confundirse:

- No ejecuta exploits
    
- No valida si la máquina es vulnerable
    
- No descarga payloads mágicos
    
- No hace fingerprint de versiones
    

Searchsploit solo **da la referencia** para que tú armes el ataque con Metasploit, Python o GCC.

---

## Flujo táctico real en un pentest

1. Descubres versión
    
2. Buscas en Searchsploit
    
3. Copias exploit si hay
    
4. Lo ajustas (IP, puerto, offset, etc)
    
5. Atacas con tu herramienta preferida
    

Ejemplo real con Samba:

`nmap -sV <ip> searchsploit samba 3.0.20 searchsploit -m linux/remote/9934.sh`

Luego lo editas y ejecutas con `sh`.

---

## Integración con Metasploit

Searchsploit te dice si existe módulo MSF cuando aparece algo como:

`(CVE-YYYY-XXXX) (MSF)`

Entonces puedes:

`msfconsole search <cve>`