---
title: "Como Instalar Cisco Packet Tracer 7.3.x en RHEL 8 / CentOS 8 / Fedora 32 sin alien"
date: 2020-09-29
lastmod: 2020-09-29
author: Enmanuel Moreira
description: "En el siguiente tutorial vamos a instalar Packet Tracer 7.3.1 en CentOS 8, sin embargo es válido también para sistemas con RHEL 8 y Fedora 32. Es probable que se necesite instalar algunas bibliotecas adicionales y que varien de distribución en distribución."
draft: false

resources:
- name: "featured-image"
  src: "featured-image.png"

categories: ["Linux","Redes"]
tags: ["tutorial","redes","cisco"]

lightgallery: true
---

<!--more-->

## PROMO DigitalOcean

***

Antes de comenzar, quería contarles que hay una promoción en DigitalOcean donde te dan un crédito de USD 100.00 durante 60 días para que puedas probar los servicios que este Proveedor Cloud ofrece. Lo único que tienes que hacer es suscribirte a DigitalOcean con el siguiente enlace: <https://bit.ly/digitalocean-itsm>

***

¡Hola a todos!  

En el siguiente tutorial vamos a instalar Packet Tracer 7.3.1 en CentOS 8, sin embargo es válido también para sistemas con RHEL 8 y Fedora 32. Es probable que se necesite instalar algunas bibliotecas adicionales y que varien de distribución en distribución.  

Ahora, ¿por qué esto seria necesario? Cisco siempre ha soportado unicamente a Ubuntu para la distribución del empaquetado de su software, aunque tambien habia la opción de instalarlo desde un .tar.gz. Desafortunadamente, Cisco anunció que Packet Tracer solo iba a estar disponible como paquete .deb a partir de la versión 7.3 y que solo seria soportado oficialmente Ubuntu 18.04.03 LTS.  

Es posible convertir el paquete .deb a uno .rpm con la utilidad **alien**, pero, con los pasos que les voy a indicar a continuación, no será necesario instalar alien en nuestro sistema.  

## Descargando el Instalador .deb

***

Para descargar el paquete .deb, debemos iniciar sesión con nuestro usuario y clave en la página de [Cisco Netacad](https://www.netacad.com), e ir al menu Recursos -> Descargar Packet Tracer:  

![Pantalla Netacad](/images/instalar-packet-tracer-rpm/pt-0.png "Pantalla Netacad")

![Pantalla Descargas](/images/instalar-packet-tracer-rpm/pt-1.png "Pantalla Descargas")

## Desempaquetando el Instalador

***

Ahora procedemos a desempaquetar el archivo .deb:  

- Creamos un directorio en /tmp donde va a almacenar los archivos desempaquetados:  

```bash
$ mkdir /tmp/pt-instalador
```

- Copiamos el instalador .deb al directorio recien creado y entramos al mismo:  

```bash
$ cp PacketTracer_731_amd.deb /tmp/pt-instalador
$ cd /tmp/pt-instalador
```

- Desempaquetamos:  

```bash
$ ar -xv PacketTracer_731_amd.deb
```

- Creamos los directorios control y data:  

```bash
$ mkdir /tmp/pt-instalador/{control,data}
```

- Descomprimimos los archivos control y data (que estabana dentro del archivo .deb):  

```bash
$ tar -C control -xfJ control.tar.xz
$ tar -C data -xfJ data.tar.xz
```

- Entramos al directorio data:  

```
$ cd data
```

## Eliminando Instalación Antigua de PT

En caso de tener una versión anterior de Packet Tracer instalada, vamos a borrar el directorio donde se encuentra instalado (asumiendo que se encuentren en el directorio /opt/pt) y los archivos de la aplicación (accesos directos, iconos, etc):  

```bash
$ sudo rm -rf /opt/pt
$ sudo rm -rf /usr/share/applications/cisco-pt7.desktop
$ sudo rm -rf /usr/share/applications/cisco-ptsa7.desktop
$ sudo rm -rf /usr/share/icons/hicolor/48x48/apps/pt7.png
```

## Instalando y Configurando Packet Tracer

Asegurandonos que aún estamos en el directorio /tmp/pt-instalador, procedemos a copiar los directorios:  

```bash
$ sudo cp -r usr /
$ sudo cp -r opt /
```

Ahora actualizamos las asociaciones de los iconos y los archivos para que los sistema reconozca:  

```bash
$ sudo xdg-desktop-menu install /usr/share/applications/cisco-pt7.desktop
$ sudo xdg-desktop-menu install /usr/share/applications/cisco-ptsa7.desktop
$ sudo update-mime-database /usr/share/mime
$ sudo gtk-update-icon-cache --force --ignore-theme-index /usr/share/icons/gnome
$ sudo xdg-mime default cisco-ptsa7.desktop x-scheme-handler/pttp
```

Creamos un enlace simbólico del ejecutable de Packet Tracer a /usr/local/bin:  

```bash
$ sudo ln -sf /opt/pt/packettracer /usr/local/bin/packettracer
```

Para finalizar, vamos a actualizar el archivo /etc/profile para apuntar al directorio /opt/pt:  

```bash
$ sudo vim /etc/profile
```

Y pegamos al final del archivo lo siguiente:  

```bash
PT7HOME=/opt/pt
export PT7HOME
QT_DEVICE_PIXEL_RATIO=auto
export QT_DEVICE_PIXEL_RATIO
```

Cerramos sesión, e iniciamos sesión nuevamente para que tome los cambios del perfil. Comprobamos que esté actualizado, haciendo una llamada a la variable de entorno PT7HOME y QT_DEVICE_PIXEL_RATIO:  

```bash
$ echo $PT7HOME
/opt/pt
```

```bash
$ echo $QT_DEVICE_PIXEL_RATIO
auto
```

En la consola, ejecutamos el comando packettracer o en el menu principal veremos el icono:

```bash
$ packettracer
```

![Pantalla APP](/images/instalar-packet-tracer-rpm/pt-2.png "Pantalla Principal Packet Tracer")

## Troubleshooting

En caso que Packet Tracer no inicie, vamos a copiar el script que llama al ejecutable packettracer para poder debuguear la aplicacion:  

```bash
$ sudo cp /opt/pt/packettracer /opt/pt/ptdebug
```

Editamos el archivo /opt/pt/ptdebug, vamos a quitarle el redireccionamiento a /dev/null para ver los mensajes de la aplicación por la consola:  

```bash
$ sudo vim /opt/pt/ptdebug
```

Archivo Original:

```bash
#!/bin/bash

echo Starting Packet Tracer 7.3.1

PTDIR=/opt/pt
export LD_LIBRARY_PATH=/opt/pt/bin
pushd /opt/pt/bin > /dev/null
./PacketTracer7 "$@" > /dev/null 2>&1
popd > /dev/null
```

Archivo Modificado:  

```bash
#!/bin/bash

echo Starting Packet Tracer 7.3.1

PTDIR=/opt/pt
export LD_LIBRARY_PATH=/opt/pt/bin
pushd /opt/pt/bin
./PacketTracer7 "$@"
popd
```

Ejecutamos ptdebug:  

```bash
$ sh /opt/pt/ptdebug
Starting Packet Tracer 7.3.1
/opt/pt/bin ~
Qt WebEngine ICU data not found at /opt/pt/bin/resources. Trying parent directory...
Installed Qt WebEngine locales directory not found at location /opt/pt/bin/translations/qtwebengine_locales. Trying application directory...
Qt WebEngine resources not found at /opt/pt/bin/resources. Trying parent directory...
```

Con ptdebug podremos ver si le faltan bibliotecas por instalar y actuamos en consecuencia.  

Bueno esto es todo por hoy, espero les haya gustado. ¡Hasta la próxima!

Si te pareció útil este artículo, puedes brindarme un café :)

<style>.bmc-button img{height: 34px !important;width: 35px !important;margin-bottom: 1px !important;box-shadow: none !important;border: none !important;vertical-align: middle !important;}.bmc-button{padding: 7px 15px 7px 10px !important;line-height: 35px !important;height:51px !important;text-decoration: none !important;display:inline-flex !important;color:#ffffff !important;background-color:#5F7FFF !important;border-radius: 8px !important;border: 1px solid transparent !important;font-size: 24px !important;letter-spacing: 0.6px !important;box-shadow: 0px 1px 2px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;margin: 0 auto !important;font-family:'Cookie', cursive !important;-webkit-box-sizing: border-box !important;box-sizing: border-box !important;}.bmc-button:hover, .bmc-button:active, .bmc-button:focus {-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;text-decoration: none !important;box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;opacity: 0.85 !important;color:#ffffff !important;}</style><link href="https://fonts.googleapis.com/css?family=Cookie" rel="stylesheet"><a class="bmc-button" target="_blank" href="https://www.buymeacoffee.com/enmanuelmoreira"><img src="https://cdn.buymeacoffee.com/buttons/bmc-new-btn-logo.svg" alt="Buy me a coffee"><span style="margin-left:5px;font-size:24px !important;">Buy me a coffee</span></a>
