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

Hola chiquillos!

El día de hoy les quería comentar de una herramienta que conocí a través del sitio web **[4Fasters](https://4fasters.com.br)**: **[Notion](https://notion.so/)**  

El site de **[Notion](https://notion.so/)** define a la herramienta como:  
> Is a collaboration platform with markdown support that integrates kanban boards, tasks, wikis, and databases. The company claims to be an all-in-one workspace for note-taking, knowledge and data management, project and task management.  

Notion es una excelente herramienta para organizar nuestra vida, es tan potente que nos permite llevar un registro de las tareas, realizar notas, hacer graficos, etc. Lo mejor es que los cambios se sincronizan pudiendolos accesar desde cualquier lugar en cualquier momento.  

Hay dos versiones: Una personal y otra de Pago. Los precios de la suscripción se pueden ver en este **[enlace](https://www.notion.so/pricing)**  

Es compatible con Microsoft Windows, MacOS, Android e iOS (además de tener una versión Web) Lamentablemente, aún no han lanzado una versión de escritorio oficial para Linux y según lo que podemos leer de la cuenta de **[Twitter](https://twitter.com/NotionHQ)** no lo van a hacer en un futuro cercano.  

{{< tweet 912737143327301634 >}}

Afortunadamente, a través de Electon y apoyándonos en un script, podemos compilar e instalar Notion en Fedora 32 partiendo del instalador para Microsoft Windows.

### Paso 1: Instalamos Node.js y algunas dependencias  

```bash
sudo dnf install nodejs p7zip-plugins ImageMagick fakeroot -y
```

### Paso 2: Instalamos dependencias desde npm

```bash
sudo npm -g install asar electron-packager electron-installer-redhat electron-installer-debian
```

### Paso 3: Clonamos el repositorio notion-rpm-builder

```bash
git clone https://github.com/enmanuelmoreira/notion-rpm-builder
```

Ahora nos cambiamos a la carpeta

```bash
cd notion-rpm-builder
```

### Paso 4: Descargamos la última versión de Notion

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

Solo nos queda instalar el paquete notion-desktop

```bash
sudo rpm -i dist/installers/notion-desktop-2.0.8-1.x86_64.rpm
```

Y listo ya lo tendremos funcionando en nuestro sistema.  

![Notion Desktop](/images/notion-fedora-32/notion-desktop.png "Pantalla Principal")  

Espero les haya gustado ¡Hasta la próxima!
