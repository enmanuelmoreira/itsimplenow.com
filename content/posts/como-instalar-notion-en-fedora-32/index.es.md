---
title: "Como instalar Notion en Fedora 32"
date: 2020-08-01
lastmod: 2020-08-01
author: "Enmanuel Moreira"
description: "En este artículo vamos a instalar la herramienta Notion en Fedora 32" 
draft: false

resources:
- name: "featured-image"
  src: "featured-image.jpg"

categories: ["Linux"]
tags: ["linux", "fedora","tutoriales"]

lightgallery: true
---

<!--more-->

## PROMO DigitalOcean

***

Antes de comenzar, quería contarles que hay una promoción en DigitalOcean donde te dan un crédito de USD 100.00 durante 60 días para que puedas probar los servicios que este Proveedor Cloud ofrece. Lo único que tienes que hacer es suscribirte a DigitalOcean con el siguiente enlace: <https://bit.ly/digitalocean-itsm>

***

## Introducción

***

¡Hola a todos!

El día de hoy les quería comentar de una herramienta que conocí a través del sitio web **[4Fasters](https://4fasters.com.br)**: **[Notion](https://notion.so/)**  

El site de **[Notion](https://notion.so/)** define a la herramienta como:  
> Is a collaboration platform with markdown support that integrates kanban boards, tasks, wikis, and databases. The company claims to be an all-in-one workspace for note-taking, knowledge and data management, project and task management.  

Notion es una excelente herramienta para organizar nuestra vida, es tan potente que nos permite llevar un registro de las tareas, realizar notas, hacer gráficos, etc. Lo mejor es que los cambios se sincronizan pudiendolos accesar desde cualquier lugar en cualquier momento.  

Hay dos versiones: Una personal y otra de Pago. Los precios de la suscripción se pueden ver en este **[enlace](https://www.notion.so/pricing)**  

Es compatible con Microsoft Windows, MacOS, Android e iOS (además de tener una versión Web) Lamentablemente, aún no han lanzado una versión de escritorio oficial para Linux y según lo que podemos leer de la cuenta de **[Twitter](https://twitter.com/NotionHQ)** no lo van a hacer en un futuro cercano.  

{{< tweet 912737143327301634 >}}

Afortunadamente, a través de Electon y apoyándonos en un script, podemos compilar e instalar Notion en Fedora 32 partiendo del instalador para Microsoft Windows.

### Paso 1: Instalamos Node.js y algunas dependencias  

***

```bash
sudo dnf install nodejs p7zip-plugins ImageMagick fakeroot -y
```

### Paso 2: Instalamos dependencias desde npm

***

```bash
sudo npm -g install asar electron-packager electron-installer-redhat electron-installer-debian
```

### Paso 3: Clonamos el repositorio notion-rpm-builder  

***

```bash
git clone https://github.com/enmanuelmoreira/notion-rpm-builder
```

Ahora nos cambiamos a la carpeta

```bash
cd notion-rpm-builder
```

### Paso 4: Descargamos la última versión de Notion

***

A día de hoy es la 2.0.8  

```bash
wget 'https://desktop-release.notion-static.com/Notion%20Setup%202.0.8.exe' -O notion.exe
```

Le damos permisos de ejecución al script build.sh  

```bash
chmod +x build.sh
```

Ejecutamos el script:  

```bash
./build.sh <platform>
```

Donde se va a reemplazar **`<platform>`** por la versión del sistema operativo del instalador (en este caso Windows)  

```bash
./build.sh windows
```

Esperamos pacientemete unos minutos, y el paquete debería estar en la carpeta dist/installers

### Paso 5: Instalamos el paquete contruido  

***

Solo nos queda instalar el paquete notion-desktop

```bash
sudo rpm -i dist/installers/notion-desktop-2.0.8-1.x86_64.rpm
```

Y listo ya lo tendremos funcionando en nuestro sistema.  

![Notion Desktop](/images/notion-fedora-32/notion-desktop.png "Pantalla Principal")  

Espero les haya gustado ¡Hasta la próxima!

## Apoya este Proyecto!!!

Si te pareció útil este artículo y el proyecto en general, considera brindarme un café :)

<style>.bmc-button img{height: 34px !important;width: 35px !important;margin-bottom: 1px !important;box-shadow: none !important;border: none !important;vertical-align: middle !important;}.bmc-button{padding: 7px 15px 7px 10px !important;line-height: 35px !important;height:51px !important;text-decoration: none !important;display:inline-flex !important;color:#ffffff !important;background-color:#5F7FFF !important;border-radius: 8px !important;border: 1px solid transparent !important;font-size: 24px !important;letter-spacing: 0.6px !important;box-shadow: 0px 1px 2px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;margin: 0 auto !important;font-family:'Cookie', cursive !important;-webkit-box-sizing: border-box !important;box-sizing: border-box !important;}.bmc-button:hover, .bmc-button:active, .bmc-button:focus {-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;text-decoration: none !important;box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;opacity: 0.85 !important;color:#ffffff !important;}</style><link href="https://fonts.googleapis.com/css?family=Cookie" rel="stylesheet"><a class="bmc-button" target="_blank" href="https://www.buymeacoffee.com/enmanuelmoreira"><img src="https://cdn.buymeacoffee.com/buttons/bmc-new-btn-logo.svg" alt="Buy me a coffee"><span style="margin-left:5px;font-size:24px !important;">Buy me a coffee</span></a>
