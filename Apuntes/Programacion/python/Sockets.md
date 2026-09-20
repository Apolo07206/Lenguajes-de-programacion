---
title: Sockets
area: programacion
tipo: lenguaje
tags: [programacion, python]
created: 2026-09-18
updated: 2026-09-20
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Sockets]]", "[[redes y protocolos]]", "[[apuntes normales]]"]
---

## ¿ Para que sirbe sockets ?

sockets sirve para establece una conexion entre un servidor ( quien recibe las conexiones ) y un cliente ( quien envia las conexiones )

# Conceptos importantes que debes recordar

### 🔹 `bind()`

Asocia el servidor a una IP y puerto.

### 🔹 `listen()`

Pone al servidor en modo espera.

### 🔹 `accept()`

Crea un nuevo socket exclusivo para ese cliente.

### 🔹 `send()` y `recv()`

Siempre trabajan con **bytes**, por eso usamos:

`.encode() .decode()`

### 🔹 Flujo correcto en TCP

Servidor:

`send → recv → send`

Cliente:

`recv → send → recv`

## Ejemplo de servidor 

``` python

import socket

# Clave correcta que el servidor validará
clave = "ferney123"

def servidor():
    # 1️⃣ Crear el socket
    # AF_INET  → IPv4
    # SOCK_STREAM → TCP
    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

    # 2️⃣ Asociar el socket a una IP y puerto
    # 0.0.0.0 significa: escuchar en todas las interfaces de red
    server.bind(("0.0.0.0", 9100))

    # 3️⃣ Poner el servidor en modo escucha
    # El número indica cuántas conexiones pueden estar en cola
    server.listen(1)

    print("Esperando conexion ....")

    # 4️⃣ Esperar a que un cliente se conecte
    # accept() devuelve:
    # - client → nuevo socket para comunicarse con ese cliente
    # - addr → dirección IP y puerto del cliente
    client, addr = server.accept()
    print("Cliente conectado:", addr)

    try:
        # 5️⃣ Banner ASCII que se enviará al cliente
        banner = r"""
███████╗███████╗██████╗ ██╗   ██╗███████╗██████╗ 
██╔════╝██╔════╝██╔══██╗██║   ██║██╔════╝██╔══██╗
███████╗█████╗  ██████╔╝██║   ██║█████╗  ██████╔╝
╚════██║██╔══╝  ██╔══██╗╚██╗ ██╔╝██╔══╝  ██╔══██╗
███████║███████╗██║  ██║ ╚████╔╝ ███████╗██║  ██║
╚══════╝╚══════╝╚═╝  ╚═╝  ╚═══╝  ╚══════╝╚═╝  ╚═╝
void main
            >>> SERVER HACKING <<<

Ingresa la clave:
"""

        # 6️⃣ Enviar el banner al cliente
        # encode() convierte string → bytes (obligatorio en sockets)
        client.send(banner.encode())

        # 7️⃣ Recibir datos del cliente
        # recv(4096) significa: recibir hasta 4096 bytes
        # decode() convierte bytes → string
        # strip() elimina espacios y saltos de línea
        clave_recibida = client.recv(4096).decode().strip()

        # 8️⃣ Validar la clave
        if clave_recibida == clave:
            client.send("\n[+] ACCESO CONCEDIDO\n".encode())
        else:
            client.send("\n[-] ACCESO DENEGADO\n".encode())

    # 9️⃣ Manejar error si el cliente se desconecta abruptamente
    except ConnectionResetError:
        print("El cliente cerro la conexion inesperadamente.")

    # 🔟 Cerrar conexiones correctamente
    finally:
        client.close()  # cerrar conexión con cliente
        server.close()  # cerrar servidor
        print("Servidor cerrado.")


# Ejecutar servidor
servidor()


```


## Ejemplo de cliente 


``` python

import socket

  

def cliente():

cliente = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

cliente.connect(("192.168.1.10", 9100)) # IP del servidor

  

# 1️⃣ Recibir banner

respuesta = cliente.recv(4096).decode()

print(respuesta)

  

# 2️⃣ Pedir clave

clave = input(">>> ")

  

# 3️⃣ Enviar clave

cliente.send(clave.encode())

  

# 4️⃣ Recibir resultado

resultado = cliente.recv(4096).decode()

print(resultado)

  
cliente.close()


cliente()

```

## Ver también

- [[MOC-programacion]]
- [[Ruta Python]]
- [[Sockets]]
- [[redes y protocolos]]
- [[apuntes normales]]
