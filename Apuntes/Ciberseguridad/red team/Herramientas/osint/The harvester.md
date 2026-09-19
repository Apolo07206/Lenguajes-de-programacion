Herramienta OSINT para recolectar información pública de objetivos específicos (normalmente dominios). Saca correos, subdominios, hosts, nombres y otros datos desde buscadores, servicios públicos y motores especiales.

No ataca. Solo junta datos para abrir rutas.


https://youtu.be/g9mQzg0Yxbw?si=W1YjFQ8GO-XxqPOU
---

## Para qué sirve

Útil en:

- Reconocimiento temprano en pentesting
    
- Enumeración previa a red teaming
    
- Doxing corporativo legal
    
- Mapeo de superficie de exposición
    
- CTFs OSINT
    
- Blue Team para autoevaluar exposición
    

Te proporciona:

- Correos de empleados
    
- Subdominios
    
- IPS asociadas
    
- Nombres
    
- Hosts públicos
    
- Fuentes donde fue encontrado el dato
    

---

## Instalación útil

En Kali ya viene. Si no:

`git clone https://github.com/laramies/theHarvester.git cd theHarvester pip3 install -r requirements.txt`

---

## Uso básico

Formato principal:

`theHarvester -d <dominio> -b <fuente>`

Ejemplo:

`theHarvester -d microsoft.com -b google`

Fuentes múltiples separadas por coma:

`theHarvester -d tesla.com -b google,bing,duckduckgo`

---

## Fuentes útiles

El parámetro `-b` define la fuente. Algunas de las más potentes:

- `google`
    
- `bing`
    
- `duckduckgo`
    
- `crtsh`
    
- `rapiddns`
    
- `shodan` (requiere API)
    
- `certspotter`
    
- `hunter` (requiere API)
    
- `linkedin` (requiere métodos externos)
    
- `reddit` (para leaks y menciones)
    
- `urlscan`
    

Puedes ver todas las fuentes disponibles con:

`theHarvester -h`

---

## Output y formatos

Exportar a archivo:

`theHarvester -d target.com -b google -f reporte`

Esto generará:

- `reporte.xml`
    
- `reporte.json`
    
- `reporte.txt`
    

Dependiendo de versión y opciones.

---

## Datos valiosos que obtienes

**Correos**: útiles para spear phishing, ataques password spray, OSINT personal.

**Subdominios**: pueden revelar:

- Paneles administrativos
    
- APIs internas mal expuestas
    
- Servicios viejos
    
- Integraciones de terceros
    

**IPS y Hosts**: ayudan a mapear infraestructura:

- CDNs
    
- Cloud (AWS, Azure, GCP)
    
- Datacenters externos
    
- Servicios expuestos por error
    

**Fuentes**: sirve para verificar legitimidad.

---

## Opciones útiles

Usar API Keys (más resultados):

`theHarvester -d target.com -b bing -l 500`

Limitar resultados:

`-l <cantidad>`

Enumerar hosts al vuelo:

`theHarvester -d target.com -b google -n`

IPs con DNS reverso:

`theHarvester -d target.com -b google -c`

Modo silencioso:

`-s`

---

## Ejemplo realista de reconocimiento corporativo

Objetivo: `empresa.com`

1. Recolección inicial
    
    `theHarvester -d empresa.com -b google,bing,crtsh`
    
2. Revisión de correos
    
    - Extraes emails de empleados
        
3. Subdominios a `dnsenum` o `amass`
    
4. Buscar assets vulnerables en Shodan con:
    
    `theHarvester -d empresa.com -b shodan`
    
5. Correos filtrados a `h8mail` o `HIBP`
    

Resultado: mapa mental de exposición real.

---

## Limitaciones reales

No puede:

- Bypassear autenticación
    
- Extraer datos privados
    
- Enumerar en redes internas
    
- Validar vulnerabilidades
    
- Automatizar spear phishing
    

Depende de:

- Calidad de fuentes
    
- API keys
    
- Políticas de scraping
    
- Restricciones por región
    

---

## Integraciones potentes

Combina como un puzzle:

- `amass` para subdominios brutales
    
- `subfinder` para cobertura amplia
    
- `httpx` para verificar hosts vivos
    
- `shodan` para exposición de servicios
    
- `censys` para certificados
    
- `SpiderFoot` para automatización
    
- `Mailfy/HIBP` para leaks
    

Ejemplo:

`theHarvester -> amass -> httpx -> nuclei`