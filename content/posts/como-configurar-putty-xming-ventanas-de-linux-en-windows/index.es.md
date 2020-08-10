---
title: "Como Configurar Xming y Redireccionar Ventanas X11 en Microsoft Windows"
date: 2020-07-24
lastmod: 2020-07-24
author: "Enmanuel Moreira"
description: "En este articulo vamos a configurar el redireccionamiento de X11 a Microsoft Windows, usando Xming y SSH."
draft: false

resources:
- name: "featured-image"
  src: "featured-image.png"

categories: ["Linux"]
tags: ["linux","tutoriales"]

lightgallery: true
---

<!--more-->

Hola chiquillos!

Hablando hace días con un amigo SysAdmin, tenía una necesidad bastante específica: el software que estaba instalando en un servidor era escrito en Java, y para configurar los parámetros del mismo, necesitaba SI o SI abrir la ventana del configurador, no obstante, el servidor que estaba instalando SORPRESAAAAA!!! no tenia la interfaz gráfica instalada (por obvias razones en un servidor de producción) y que él no encontraba la manera de hacerlo por la linea de comandos. La documentación del software en cuestión decía explícitamente que se debía abrir el configurador por X11.

Entonces, ¿que hacer? Afortunadamente, llevo bastante tiempo utilizando dos herramientas para lograr redireccionar (o ver en términos más simples) una ventana de Linux (X11) en Microsoft Windows. Estas dos herramientas son Xming y Putty. En este tutorial les voy a explicar como hacerlo.

**[Xming](https://sourceforge.net/projects/xming/files/Xming/)** es un software desarrollado por Colin Harrison siendo el servidor lider de X11 para Microsoft Windows. Tiene muchas funciones, es simple, rápido, fácil de instalar (lo veremos en un momento) y, dado que es un Windows nativo independiente, se hace fácilmente portátil (no necesita una instalación específica de la máquina ni acceso al registro de Windows).

Xming es totalmente seguro cuando se usa con SSH y opcionalmente incluye un cliente Plink SSH mejorado y un paquete portátil de reemplazo de PuTTY. Los instaladores de Xming incluyen código ejecutable y bibliotecas. Era open source hasta el año 2007 y su última versión fue la 6.9.0.31. Actualmente, es de código cerrado y se puede conseguir en **<http://www.straightrunning.com/XmingNotes/>** (ya va en su versión 7.7.0.56) con una donación de 10 libras esterlinas (aproximadamente unos USD 10.00) y puedes recibir un año de actualizaciones.

**[Putty](https://www.putty.org/)** es un cliente SSH y Telnet, que fue desarrollado originalmente por Simon Tatham para la plataforma Microsoft Windows. PuTTY es un software open source cuyo código fuente está disponible para cualquiera que quiera aportar a mejorar la herramienta, además de ser ampliamente soportado por una comunidad de voluntarios.

Ya con estas dos herramientas, además de una PC o Máquina Virtual con Linux (CentOS 8) y un PC Windows 10 (en mi caso), podemos comenzar, Manos a la obra!

## Paso 1: Descargamos el software necesario en la máquina cliente

***

Procedemos a Descargar la versión actual de Putty para Windows:

* 32 bits: **<https://the.earth.li/~sgtatham/putty/latest/w32/putty-0.74-installer.msi>**
* 64 bits: **<https://the.earth.li/~sgtatham/putty/latest/w64/putty-64bit-0.74-installer.msi>**

Ahora descargamos Xming desde la página del proyecto en Sourceforge:
**<https://sourceforge.net/projects/xming/files/Xming/6.9.0.31/Xming-6-9-0-31-setup.exe/download>**

Descargamos las fuentes de Xming:
**<https://sourceforge.net/projects/xming/files/Xming-fonts/7.7.0.10/Xming-fonts-7-7-0-10-setup.exe/download>**

## Paso 2: Instalando Putty

La instalación es bastante sencilla (hay que contar con permisos de Administrador):

Click en Next

![Installer](/images/putty-xming/putty-0.png)

Podemos cambiar el directorio destino, yo lo voy a dejar como predeterminado:

![Destination Folder](/images/putty-xming/putty-1.png)

Dejamos tal cual como está:

![Product Features](/images/putty-xming/putty-2.png)

Final

![Finish](/images/putty-xming/putty-3.png)

## Paso 2: Instalando Xming

***

Iniciamos el instalador:

![Installer](/images/putty-xming/xming-0.png)

Escogemos la carpeta destino, yo lo voy a dejar predeterminado

![Destination Folder](/images/putty-xming/xming-1.png)

Seleccionamos los elementos a instalar, yo lo dejaré en Full Installation

![Menu Folder](/images/putty-xming/xming-2.png)

Click en Next

![Select Components](/images/putty-xming/xming-3.png)

Creamos los accesos directos para Xming y XLaunch (XLaunch es el configurador de Xming)

![Additionals Tasks](/images/putty-xming/xming-4.png)

Procedemos con la instalación

![Ready to Install](/images/putty-xming/xming-5.png)

Iniciamos Xming

![Finish](/images/putty-xming/xming-6.png)

En la Barra de tareas nos aparecerá un Icono X, donde está residente el Xming

![Xming icon](/images/putty-xming/xming-7.png)

## Paso 3: Instalando Xming-Fonts

***

Click en Next

![Select Components](/images/putty-xming/xfonts-0.png)

Click en Install

![Ready to Install](/images/putty-xming/xfonts-1.png)

Finalizamos la instalación

![Finish](/images/putty-xming/xfonts-2.png)

## Paso 4: Configuramos Xming con XLaunch

***

Abrimos XLaunch

Escogeremos la opción de multiples ventanas:

![Display Settings](/images/putty-xming/xlaunch-0.png)

Lo dejamos por defecto en solo iniciar el cliente Xming

![Session Type](/images/putty-xming/xlaunch-1.png)

Si tenemos algunos parametros adicionales, sino Next

![Additional Parameters](/images/putty-xming/xlaunch-2.png)

Si queremos guardar la configuración actual y jugar mas a futuro con las opciones, click en Save Configuration

![Finish](/images/putty-xming/xlaunch-3.png)

Ahora debemos colocar el acceso directo de Xming en C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp que es la carpeta de inicio de Windows.

## Paso 5: Modificando los valores del servidor SSH en Linux

***

Bien, ahora tenemos que ir nuestro servidor Linux y modificar con su editor de texto favorito (nano, vi), el siguente archivo /etc/sshd/sshd_config

```bash
sudo nano /etc/ssh/sshd_config
```

Descomentamos el siguiente valor.

```plaintext
# X11Forwarding no
```

y cambiamos a:

```plaintext
X11Forwarding yes
```

Guardamos los cambios y reiniciamos le servidor SSH

```bash
sudo systemctl restart sshd
```

## Paso 6: Configurando Putty

***

Lo primero que tenemos que hacer, es ir al Menú Connection -> SSH -> X11 y activamos la opción Enable X11 forwarding:

![Putty X11 forwarding](/images/putty-xming/putty-4.png)

Ahora vamos al menú Session, y ahí colocamos la Dirección IP de nuestro servidor, un nombre para él y hacemos click en Save

![Putty Session](/images/putty-xming/putty-5.png)

Hacemos click en Load (para cargar la configuración) y ahora Open

Nos aparecerá este mensaje, donde nos indica que la llave ssh del servidor no está registrada dentro de los hosts conocidos (como es una prueba de concepo no importa mucho, sin embargo, procura siempre autenticarte con tus propias llaves), hacemos click en Sí

![Putty keys](/images/putty-xming/putty-6.png)

Iniciamos sesión con nuestro usuario y contraseña:

![Putty login](/images/putty-xming/putty-7.png)

Ahora iniciemos el programa glxgears como ejemplo:

![Putty glxgears](/images/putty-xming/putty-8.png)

Ahora Firefox:

![Putty Firefox](/images/putty-xming/putty-9.png)

Documentación: **<http://www.straightrunning.com/XmingNotes/manual.php>**

Espero les haya gustado este tutorial, ¡hasta la próxima!
