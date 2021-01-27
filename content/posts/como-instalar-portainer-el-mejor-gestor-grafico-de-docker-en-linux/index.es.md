---
title: "Como Instalar Portainer: El Mejor Gestor Gráfico de Docker en Linux"
date: 2021-01-28
lastmod: 2021-01-28
author: Enmanuel Moreira
description: "En un articulo anterior, vimos como instalar Docker en las distribuciones Linux más utilizadas, vimos unos comandos básicos que nos permitian poder descargar una imagen de Docker y ejecutarla (la imagen de prueba HelloWorld), sin embargo, para quien no está acostumbrado a la terminal, la curva de aprendizaje de Docker seria realmente dificil por lo que en este tutorial vamos a instalar una interfaz gráfica que nos va a salvar la vida: Portainer"
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

En un articulo anterior [Como Instalar Docker en Linux](https://www.itsimplenow.com/como-instalar-docker-en-linux/), vimos como instalar Docker en las distribuciones Linux más utilizadas, vimos unos comandos básicos que nos permitian poder descargar una imagen de Docker y ejecutarla (la imagen de prueba HelloWorld), sin embargo, para quien no está acostumbrado a la terminal, la curva de aprendizaje de Docker seria realmente dificil por lo que en este tutorial vamos a instalar una interfaz gráfica que nos va a salvar la vida: **[Portainer](https://www.portainer.io/)**.  

**[Portainer](https://www.portainer.io/)** es una interfaz gráfica web liviana que nos va a permitir gestionar contenedores, imágenes, servicios, etc y nos permitirá manejar de manera sencilla ambientes Docker (hosts Docker o clústeres de Swarm) Es desplegado en un contenedor Docker en cualquier sistema operativo donde tenga instalado el motor (Windows, macOS o Linux.  

## ¿Qué puede hacer Portainer por nosotros?

***

* Gestionar imágenes Docker – pull, delete, Build  
* Desplegar aplicaciones rápidamente desde plantillas  
* Gestionar contenedores – iniciar, detener, matar, reiniciar, pausar, continuar, eliminar, crear  
* Gestionar redes – añadir, borrar, modificar  
* Gestionar Volumenes Persistentes – añadir, eliminar, administrar privilegios  
* Revisar los eventos del motor Docker  
* Añadir un registro Docker privado y configurar la autenticación para DcokerHub  
* Crear endpoints  
* Añadir usuarios para gestionar Docker  
* Administrar Docker Swarm  
* Crear plantillas personalizadas de contenedores  

## Desplegando Portainer

***

Algunos prerrequisitos que vamos a necesitar son:  

* Un host con Linux, macOS o Windows instalado.  
* Docker instalado y ejecutándose en un host (cuyo tutorial puedes ver **[aqui](https://www.itsimplenow.com/como-instalar-docker-en-linux/)**, pudiendo ser local o remoto.  
* Conexión a Internet para descargar la imagen de Portainer.  

### Hacer que los datos del contenedor sean Persistentes

***

Debemos crear un directorio que contendrá los datos de Portainer y que de esta manera sean persistentes.  

```bash
mkdir ~/portainer
```

También se puede especificar otro directorio si es de tu gusto.  

### Ejecutamos Portainer en el contenedor Docker

***

Procedemos a descagar e iniciar el contenedor:  

```bash
docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v ~/portainer:/data portainer/portainer-ce
```

Observamos que exponemos el puerto 9000 del contenedor y vamos a acceder a través del puerto 9000 desde el host.  La opción **--name** nos va a permitir colocarle un nombre personalizado a nuestro contenedor para así identificarlo más rápido con el comando **docker ps**, la opcion **--restart=always** va a reiniciar el contenedor en caso que este muera o se reinicie el sistema, con la opción **-v** configuramos el volumen persistente y asi los datos que Portainer intercambiará los va a guardar en el directorio ~/portainer en el host.  

### Accesando a la Interfaz Web

***

Abrimos un navegador y vamos a la dirección <http://127.0.0.1:9000> (si accedemos a una máquina remota escribiríamos <http://dirip:9000>)

Nos pedirá crear un usuario administrador y proporcionar una contraseña, despues damos click en Create User.  

![Portainer UI](/images/como-instalar-portainer/portainer-0.png "Creando Usuario y Contraseña")

Nos pedirá crear un ambiente Docker, el cual puede ser local o remoto. En mi caso, estoy usando Docker en mí máquina local.  

![Modo Instalación](/images/como-instalar-portainer/portainer-1.png "Configuración Motor Contenedores")

Una vez hecho esto, seremos redireccionados a la página inicial de Portainer donde puedes comenzar a administrar Docker desde la interfaz web. La sección predeterminada contiene un resumen del número de contenedores, versión de Docker, volúmenes, redes, etc.  

![Web UI](/images/como-instalar-portainer/portainer-2.png "Página Principal")

Dando click en la sección engine, tendremos toda la información necesaria sobre Docker.  

![Portainer Docker Engine](/images/como-instalar-portainer/portainer-3.png "Portainer")

Otra sección interesante, es la App Templates. Portainer intenta hacer que el despliegue de aplicaciones en Docker sea muy fácil proveyendo un significativo número de platillas listas para usar, entre ellas se encuentran: Nginx, MariaDB, PostgreSQL, etc. Esto se encuentra disponible tanto para Windows como para Linux. Puedes buscar y desplegar un contenedor sin perder mucho tiempo.  

![Portainer App Templates](/images/como-instalar-portainer/portainer-4.png "Plantillas de Portainer")

## Referencias

***

**<https://portainer.readthedocs.io/en/latest>**

## Artículos sobre Docker

***

* [Como Instalar Docker en Linux](https://www.itsimplenow.com/como-instalar-docker-en-linux/)  
* Como Instalar Portainer: El Mejor Gestor Gráfico de Docker en Linux  

Espero les haya gustado este tutorial, ¡hasta la próxima!

## Apoya este Proyecto!!!

Si te pareció útil este artículo y el proyecto en general, considera brindarme un café :)

<style>.bmc-button img{height: 34px !important;width: 35px !important;margin-bottom: 1px !important;box-shadow: none !important;border: none !important;vertical-align: middle !important;}.bmc-button{padding: 7px 15px 7px 10px !important;line-height: 35px !important;height:51px !important;text-decoration: none !important;display:inline-flex !important;color:#ffffff !important;background-color:#5F7FFF !important;border-radius: 8px !important;border: 1px solid transparent !important;font-size: 24px !important;letter-spacing: 0.6px !important;box-shadow: 0px 1px 2px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;margin: 0 auto !important;font-family:'Cookie', cursive !important;-webkit-box-sizing: border-box !important;box-sizing: border-box !important;}.bmc-button:hover, .bmc-button:active, .bmc-button:focus {-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;text-decoration: none !important;box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;opacity: 0.85 !important;color:#ffffff !important;}</style><link href="https://fonts.googleapis.com/css?family=Cookie" rel="stylesheet"><a class="bmc-button" target="_blank" href="https://www.buymeacoffee.com/enmanuelmoreira"><img src="https://cdn.buymeacoffee.com/buttons/bmc-new-btn-logo.svg" alt="Buy me a coffee"><span style="margin-left:5px;font-size:24px !important;">Buy me a coffee</span></a>
