---
title: "Como Configurar un Honeypot SSH y detectar Hackers en tus Servidores"
date: 2020-12-17
lastmod: 2020-12-17
author: Enmanuel Moreira
description: "Si tienes un servidor y tu u otra persona ha iniciado sesión de manera remota por SSH, probablemente te sea muy familiar la siguiente situación: Te conectas a tu servidor, revisas los logs de autenticación y descubres que alguien ha tratado de conectarse por SSH con las credenciales por defecto muchas veces. Si tienes una contraseña fuerte y usas autenticación con clave pública, estos intentos son bastantes inofensivos, aún así es un poco molesto saber que alguien está constantemente tratando de ingresar al servidor. En este artículo te enseñaré una forma bastante divertida de configurar SSH para evitar este tipo de ataques y con suerte hacerles perder un poco de tiempo a los hackers."
draft: false

resources:
- name: "featured-image"
  src: "featured-image.jpg"

categories: ["Linux", "Seguridad"]
tags: ["linux", "ubuntu", "ssh", "seguridad"]

lightgallery: true
---

<!--more-->

## PROMO DigitalOcean

***

Antes de comenzar, quería contarles que hay una promoción en DigitalOcean donde te dan un crédito de USD 100.00 durante 60 días para que puedas probar los servicios que este Proveedor Cloud ofrece. Lo único que tienes que hacer es suscribirte a DigitalOcean con el siguiente enlace: <https://bit.ly/digitalocean-itsm>

***

## Introducción

***

Si tienes un servidor y tu u otra persona ha iniciado sesión de manera remota por SSH, probablemente te sea muy familiar la siguiente situación: Te conectas a tu servidor, revisas los logs de autenticación y descubres que alguien ha tratado de conectarse por SSH con las credenciales por defecto muchas veces. Si tienes una contraseña fuerte y usas autenticación con clave pública, estos intentos son bastantes inofensivos, aún así es un poco molesto saber que alguien está constantemente tratando de ingresar al servidor.  

En este artículo te enseñaré una forma bastante divertida de configurar SSH de evitar este tipo de ataques y con suerte hacerles perder un poco de tiempo a los hackers.  

***

## Fuerza Bruta

***

La manera en que los hackers usualmente intentar acceder a los servidores es a través de un método denominado "Fuerza Bruta", en la que tienen una lista de las contraseñas más populares (entiéndase como contraseñas no seguras) e intentará acceder mediante scripts automatizados y dado que, por defecto, SSH no tiene ningún tipo de captcha o mecanismo que expulse a los atacantes después de intentos fallidos, es muy fácil de automatizar y no requiere ningún tipo de intervención manual, por lo que el atacante puede simplemente iniciar su script y continuar con su vida mientras
el script está haciendo el trabajo sucio.  

Sin embargo, en las especificaciones de SSH incluyen algo llamado "banner" que es básicamente un texto que se muestra a todos los que intentan iniciar sesión en el servidor y no hay límite sobre cuánto tiempo puede ser y cuánto tiempo puede tomar para mostrar.  

[Chris Wellons](https://nullprogram.com/blog/2019/03/22/) tomó esta idea y desarrolló un software llamado **endlessh** en cuyo [GitHub](https://github.com/skeeto/endlessh) se encuentra el código.  

***

## ¿Cómo funciona endlessh?

***

La manera en que funciona, es basicamente, pones tu servidor SSH real en un puerto diferente (por ejemplo el 2222), luego ejecutas endlessh en el puerto 22 y pretende ser el servidor SSH real. Cada vez que alguien intenta iniciar sesión, básicamente se ve obligado a leer un mensaje de texto muy, muy largo infinitamente. Nunca habrá un timeout, no arrojará un mensaje de error y mostrará garabatos hasta que el proceso se detenga manualmente.  

Dado que la mayoría de estos ataques son automatizados, puede llevar mucho tiempo hasta que el atacante se dé cuenta de que algo salió mal. Y sí, hay muchas formas normales y aburridas de proteger tus servidores como iptables o fail2ban pero con endlessh no solamente estarías protegiendo tus servidores de los atacantes, también le estarías haciendo perder su valioso tiempo en atacar otros servidores.  

## Instalando endlessh

***

En este artículo, voy a usar Ubuntu en su versión 20.04.  

Instalamos dependencias:  

```bash
sudo apt-get install make gcc libc6-dev
```

Clonamos el repositorio con el código fuente de endlessh:  

```bash 
git clone https://github.com/skeeto/endlessh
```

Entramos al directorio:  

```bash
cd endlessh
```

Compilamos:  

```bash
make
```

Colocamos el ejecutable en el directorio /usr/local/bin:  

```bash
sudo cp endlessh /usr/local/bin
```

Copiamos el archivo de configuración del servicio endlessh:  

```bash
sudo cp util/endlessh.service /etc/systemd/system
```

Creamos el directorio donde va a ir la configuración del servicio:  

```bash
sudo mkdir -p /etc/endlessh
```

Creamos el archivo de configuración y colocamos el puerto 22:  

```bash
sudo echo "Port 22" | sudo tee /etc/endlessh/config
```

Iniciamos el servicio y lo configuramos para que arranque durante el inicio del sistema operativo:  

```bash
sudo systemctl enable --now endlessh
```

Verificamos que el servicio esté escuchando:  

```bash
netstat -tupln | grep endlessh
```

Ahora, abrimos una nuevo terminal e intentemos ingresar a nuestro servidor, como puedes ver, si especifico el puerto 2222, que es el puerto en el cual el servidor ssh real está ejecutándose, está todo bien, está iniciando sesión, no hay problema, pero si configuro el puerto 22, no sucede nada.

Si colocamos la opcion:  

```bash
ssh usuario@host -p 22 -vvv
```

Veremos tras bastidores, que esas extrañas líneas aleatorias en el banner y este metraje en realidad se acelera porque en realidad cada línea tarda como 30 segundos en mostrarse y, como ya dije, el cliente realmente no trata este banner como parte del proceso de inicio de sesión, así que el atacante quedará atrapado en ese escenario.  

Espero les haya gustado este tutorial, ¡hasta la próxima!  

***

## Apoya este Proyecto!!!

Si te pareció útil este artículo y el proyecto en general, considera brindarme un café :)

<style>.bmc-button img{height: 34px !important;width: 35px !important;margin-bottom: 1px !important;box-shadow: none !important;border: none !important;vertical-align: middle !important;}.bmc-button{padding: 7px 15px 7px 10px !important;line-height: 35px !important;height:51px !important;text-decoration: none !important;display:inline-flex !important;color:#ffffff !important;background-color:#5F7FFF !important;border-radius: 8px !important;border: 1px solid transparent !important;font-size: 24px !important;letter-spacing: 0.6px !important;box-shadow: 0px 1px 2px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;margin: 0 auto !important;font-family:'Cookie', cursive !important;-webkit-box-sizing: border-box !important;box-sizing: border-box !important;}.bmc-button:hover, .bmc-button:active, .bmc-button:focus {-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;text-decoration: none !important;box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;opacity: 0.85 !important;color:#ffffff !important;}</style><link href="https://fonts.googleapis.com/css?family=Cookie" rel="stylesheet"><a class="bmc-button" target="_blank" href="https://www.buymeacoffee.com/enmanuelmoreira"><img src="https://cdn.buymeacoffee.com/buttons/bmc-new-btn-logo.svg" alt="Buy me a coffee"><span style="margin-left:5px;font-size:24px !important;">Buy me a coffee</span></a>
