---
title: "Como Instalar Docker en Linux"
date: 2021-01-22
lastmod: 2021-01-22
author: Enmanuel Moreira
description: "En este tutorial te voy a enseñar como instalar Docker CE (Community Edition) en distintas distribuciones de Linux. Docker es un motor de contenedores que nos permite empaquetar una aplicación y todas sus dependencias (bibliotecas) dentro de una unidad estandarizada, garantizando que siempre se va a ejecutar la misma aplicación independientemente de la máquina."
draft: false

resources:
- name: "featured-image"
  src: "featured-image.png"

categories: ["Linux","Contenedores"]
tags: ["redhat","centos","ubuntu","debian","opensuse","contenedores","infraestructura"]

lightgallery: true
---

<!--more-->

## PROMO DigitalOcean

***

Antes de comenzar, quería contarles que hay una promoción en DigitalOcean donde te dan un crédito de USD 100.00 durante 60 días para que puedas probar los servicios que este Proveedor Cloud ofrece. Lo único que tienes que hacer es suscribirte a DigitalOcean con el siguiente enlace: <https://bit.ly/digitalocean-itsm>

## Introducción

***

En este tutorial te voy a enseñar como instalar Docker CE (Community Edition) en distintas distribuciones de Linux.

Docker es un motor de contenedores que nos permite empaquetar una aplicación y todas sus dependencias (bibliotecas) dentro de una unidad estandarizada, garantizando que siempre se va a ejecutar la misma aplicación independientemente de la máquina.  

Docker cuenta con dos versiones: Docker CE (Community Edition) mantenida por Docker y la comunidad de software libre, y Docker EE (Enterprise Edition) de pago. Nos vamos a concentrar en instalar la version CE.  

La diferencia fundamental entre Docker y un Hypervisor, es que mientras el hypervisor virtualiza el hardware físico para instalar un sistema operativo (con su kernel, controladores, bibliotecas, aplicaciones, etc), Docker añade una capa de abstracción más y "virtualiza" el sistema operativo compartiendo el kernel del host donde esté instalado y empaquetando tanto las bibliotecas como los ejecutables del S.O.  

!["Docker vs. VM"](/images/como-instalar-docker-linux/containers-vs-virtual-machines.jpg)

## Ubuntu Linux 20.04

***

Vamos a desinstalar las antiguas versiones de Docker que se encuentren en el sistema:  

```bash
sudo apt update
sudo apt remove docker docker-engine docker.io 2>/dev/null
```

El paquete Docker CE se llama ahora docker-ce. Actualizamos la lista de paquetes:

```bash
sudo apt update
```

Instalamos algunas dependencias necesarias para el correcto funcionamiento de Docker:

```bash
sudo apt -y install apt-transport-https ca-certificates curl software-properties-common
```

Instalamos la llave GPG oficial de Docker:

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Añadimos el repositorio oficial:

```bash
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

Instalamos Docker:

```bash
sudo apt update
sudo apt -y install docker-ce docker-ce-cli containerd.io
```

Por defecto, el comando docker solo se puede ejecutar en modo root, añadiendo nuestro usuario actual al grupo "docker" vamos a poder ejecutar contenedores, descargar imágenes, ver los contenedores corriendo, etc:  

```bash
sudo usermod -aG docker $USER
```

Ahora iniciamos el servicio docker y además lo habilitamos para que inicie al arrancar el sistema:  

```bash
sudo systemctl enable --now docker
```

## Debian 10

***

Actualizamos la lista de paquetes:

```bash
sudo apt update
```

Instalamos algunas dependencias necesarias para el correcto funcionamiento de Docker:

```bash
sudo apt-get install apt-transport-https ca-certificates curl gnupg2 software-properties-common
```

Instalamos la llave GPG oficial de Docker:

```bash
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
```

Añadimos el repositorio oficial:

```bash
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
```

Instalamos Docker:

```bash
sudo apt-get update
sudo apt-get -y install docker-ce docker-ce-cli containerd.io
```

Por defecto, el comando docker solo se puede ejecutar en modo root, añadiendo nuestro usuario al grupo "docker" vamos a poder ejecutar contenedores, descargar imágenes, ver los contenedores corriendo, etc:  

```bash
sudo usermod -aG docker $USER
```

Ahora iniciamos el servicio docker y además lo habilitamos para que inicie al arrancar el sistema:  

```bash
sudo systemctl enable --now docker
```

## Fedora 33

Desinstalamos las versiones anteriores de Docker:  

```bash
sudo dnf remove docker docker-common docker-selinux docker-engine-selinux docker-engine 2>/dev/null
```

Vamos a añadir y configurar el repositorio oficial de Docker:  

```bash
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
```

Instalamos Docker CE:  

```bash
sudo dnf install docker-ce docker-ce-cli containerd.io --nobest --allowerasing
```

Por defecto, el comando docker solo se puede ejecutar en modo root, añadiendo nuestro usuario al grupo "docker" vamos a poder ejecutar contenedores, descargar imágenes, ver los contenedores corriendo, etc:  

```bash
sudo usermod -aG docker $USER
```

Ahora iniciamos el servicio docker y además lo habilitamos para que inicie al arrancar el sistema:  

```bash
sudo systemctl enable --now docker
```

## CentOS Linux 7/8

***

{{< admonition tip >}}
En CentOS 8 sustituímos el comando "yum" por "dnf"
{{< /admonition >}}

Desinstalamos versiones anteriores:  

```bash
sudo yum remove docker docker-common docker-selinux docker-engine
```

Instalamos Prerequisitos:  

```bash
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

Configuramos el repositorio oficial:  

```bash
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

Instalamos Docker CE:  

```bash
sudo yum -y install docker-ce docker-ce-cli containerd.io
```

En caso de encontrar errores con las dependencias, ejecutamos:  

```bash
sudo yum install -y --setopt=obsoletes=0 docker-ce docker-ce-selinux
```

Por defecto, el comando docker solo se puede ejecutar en modo root, añadiendo nuestro usuario al grupo "docker" vamos a poder ejecutar contenedores, descargar imágenes, ver los contenedores corriendo, etc:  

```bash
sudo usermod -aG docker $USER
```

Ahora iniciamos el servicio docker y además lo habilitamos para que inicie al arrancar el sistema:  

```bash
sudo systemctl enable --now docker
```

## OpenSUSE 15.2 / Tumbleweed

***

Afortunadamente, Docker se encuentra disponible en los repositorios oficiales de OpenSUSE. A pesar que no es la última versión, es bastante estable por lo que nos garantiza su funcionamiento.  

Actualizamos el sistema:  

```bash
sudo zypper update
```

Instalamos Docker:  

```bash
sudo zypper install docker
```

Por defecto, el comando docker solo se puede ejecutar en modo root, añadiendo nuestro usuario al grupo "docker" vamos a poder ejecutar contenedores, descargar imágenes, ver los contenedores corriendo, etc:  

```bash
sudo usermod -aG docker $USER
```

Iniciamos el servicio:  

```bash
sudo systemctl enable --now docker
```

## Probando Docker

***

To do this we will use a test image that Docker puts at our disposal. You can execute it with the following command:

```bash
docker run --rm hello-world
```

El cual debería mostrar la siguiente salida, indicádonos que puedo descargar y ejecutar el contenedor:  

```plaintext
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete 
Digest: sha256:31b9c7d48790f0d8c50ab433d9c3b7e17666d6993084c002c2ff1ca09b96391d
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

Para verificar la versión actual de Docker, ejecutamos el siguiente comando:  

```bash
docker version
```

## Artículos sobre Docker

***

* Como Instalar Docker en Linux  
* [Como Instalar Portainer: El Mejor Gestor Gráfico de Docker en Linux](https://www.itsimplenow.com/como-instalar-portainer-el-mejor-gestor-grafico-de-docker-en-linux/)  

Espero les haya gustado este tutorial, ¡hasta la próxima!

## Apoya este Proyecto!!!

Si te pareció útil este artículo y el proyecto en general, considera brindarme un café :)

<style>.bmc-button img{height: 34px !important;width: 35px !important;margin-bottom: 1px !important;box-shadow: none !important;border: none !important;vertical-align: middle !important;}.bmc-button{padding: 7px 15px 7px 10px !important;line-height: 35px !important;height:51px !important;text-decoration: none !important;display:inline-flex !important;color:#ffffff !important;background-color:#5F7FFF !important;border-radius: 8px !important;border: 1px solid transparent !important;font-size: 24px !important;letter-spacing: 0.6px !important;box-shadow: 0px 1px 2px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;margin: 0 auto !important;font-family:'Cookie', cursive !important;-webkit-box-sizing: border-box !important;box-sizing: border-box !important;}.bmc-button:hover, .bmc-button:active, .bmc-button:focus {-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;text-decoration: none !important;box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;opacity: 0.85 !important;color:#ffffff !important;}</style><link href="https://fonts.googleapis.com/css?family=Cookie" rel="stylesheet"><a class="bmc-button" target="_blank" href="https://www.buymeacoffee.com/enmanuelmoreira"><img src="https://cdn.buymeacoffee.com/buttons/bmc-new-btn-logo.svg" alt="Buy me a coffee"><span style="margin-left:5px;font-size:24px !important;">Buy me a coffee</span></a>
