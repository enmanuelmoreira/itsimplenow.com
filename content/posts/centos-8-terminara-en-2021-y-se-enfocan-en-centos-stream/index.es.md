---
title: "Soporte a CentOS 8 terminará en 2021 y se enfocan en CentOS Stream"
date: 2020-12-08
lastmod: 2020-12-08
author: Enmanuel Moreira
description: "Si alguna vez necesitas de Linux para tu empresa con un soporte oficial, Red Hat te vende el soporte para sus sistema operativo Red Hat Enterprise Linux. La mayoria de los mortales usamos una alternativa que se compila del mismo código fuente de RHEL solo que se le quitan las marcas registradas y cualquier cosa relacionada: CentOS.  "
draft: false

resources:
- name: "featured-image"
  src: "featured-image.jpg"

categories: ["Linux"]
tags: ["linux", "centos", "rhel"]

lightgallery: true
---

<!--more-->

## PROMO DigitalOcean

***

Antes de comenzar, quería contarles que hay una promoción en DigitalOcean donde te dan un crédito de USD 100.00 durante 60 días para que puedas probar los servicios que este Proveedor Cloud ofrece. Lo único que tienes que hacer es suscribirte a DigitalOcean con el siguiente enlace: <https://bit.ly/digitalocean-itsm>

***

Si alguna vez necesitas de Linux para tu empresa con un soporte oficial, Red Hat te vende el soporte para sus sistema operativo Red Hat Enterprise Linux. La mayoria de los mortales usamos una alternativa que se compila del mismo código fuente de RHEL solo que se le quitan las marcas registradas y cualquier cosa relacionada: [CentOS](https://www.centos.org/).  

Desde hace algunos años, CentOS se ha convertido en la distro basada en Red Hat más popular. En un [movimiento sorpresivo](https://blog.centos.org/2020/12/future-is-centos-stream/), CentOS está cambiando las cosas ofreciendo una versión rolling release: CentOS Stream.  

Una distro rolling release es aquella que incluye todos los componentes del sistema operativo en sus versiones más recientes. Esto conlleva a que el software no se le considera muy "maduro" (software con características muy nuevas, software no probado, bugs, etc) para entrar en un entorno de producción. Ejemplos de estas distros podrían: Arch Linux, Manjaro, OpenSUSE Tumbleweed, Fedora, etc.  

***

## Anuncio Oficial

***

Según el anuncio [oficial](https://lists.centos.org/pipermail/centos-announce/2020-December/048208.html):  

> The future of the CentOS Project is CentOS Stream, and over the next year we’ll be shifting focus from CentOS Linux, the rebuild of Red Hat Enterprise Linux (RHEL), to CentOS Stream, which tracks just ahead of a current RHEL release. CentOS Linux 8, as a rebuild of RHEL 8, will end at the end of 2021. CentOS Stream continues after that date, serving as the upstream (development) branch of Red Hat Enterprise Linux. When CentOS Linux 8 (the rebuild of RHEL8) ends, your best option will be to migrate to CentOS Stream 8, which is a small delta from CentOS Linux 8, and has regular updates like traditional CentOS Linux releases. If you are using CentOS Linux 8 in a production environment, and are concerned that CentOS Stream will not meet your needs, we encourage you to contact Red Hat about options.

En buen castellano:  

> El futuro del Proyecto CentOS es CentOS Stream, y durante el próximo año cambiaremos el enfoque de CentOS Linux, la reconstrucción de Red Hat Enterprise Linux (RHEL), a CentOS Stream, que rastrea justo antes de una versión actual de RHEL. CentOS Linux 8, como una reconstrucción de RHEL 8, finalizará a fines de 2021. CentOS Stream continúa después de esa fecha, y actúa como la rama ascendente (desarrollo) de Red Hat Enterprise Linux. Cuando finalice CentOS Linux 8 (la reconstrucción de RHEL8), su mejor opción será migrar a CentOS Stream 8, que es un pequeño delta de CentOS Linux 8, y tiene actualizaciones regulares como las versiones tradicionales de CentOS Linux. Si está utilizando CentOS Linux 8 en un entorno de producción y le preocupa que CentOS Stream no satisfaga sus necesidades, le recomendamos que se ponga en contacto con Red Hat para conocer las opciones.

***

## ¿Como serán manejados los CVE en CentOS Stream?

***

> Security issues will be updated in CentOS Stream after they are solved in the current RHEL release. Obviously, embargoed security releases can not be publicly released until after the embargo is lifted. While there will not be any SLA for timing, Red Hat Engineers will be building and testing other packages against these releases. If they do not roll in the updates, the other software they build could be impacted and therefore need to be redone. There is therefore a vested interest for them to get these updates in so as not to impact their other builds and there should be no issues getting security updates.

Si no entendiste nada hasta ahora:  

> Los problemas de seguridad se actualizarán en CentOS Stream después de que se resuelvan en la versión actual de RHEL. Obviamente, las liberaciones de seguridad embargadas no pueden publicarse hasta después de que se levante el embargo. Si bien no habrá ningún SLA para el tiempo, los ingenieros de Red Hat construirán y probarán otros paquetes con estas versiones. Si no implementan las actualizaciones, el otro software que construyen podría verse afectado y, por lo tanto, debe rehacerse. Por lo tanto, existe un gran interés para ellos en obtener estas actualizaciones para no afectar sus otras compilaciones y no debería haber problemas para obtener actualizaciones de seguridad.

Es decir, los usuarios de CentOS Stream podrán probar nuevas caracteristicas de RHEL lo que permitirá reportar fallas, sin embargo, esto no implica que van a obtener actualizaciones de seguridad por parte de RHEL. Parece muy extraño todo esto.  

***

## RHEL Beta?

***

Según el [FAQ](https://centos.org/distro-faq/)

> No. CentOS Stream will be getting fixes and features ahead of RHEL. Generally speaking, we expect CentOS Stream to have fewer bugs and more runtime features than RHEL until those packages make it into the RHEL release.

Cabe acotar que todos estos cambios no afectarán a CentOS 7 y que seguirá soportando actualizaciones de seguridad hasta 2024.  

**¿Puede la comunidad CentOS continuar desarrollando / reconstruyendo CentOS Linux?**
Red Hat dice que no pondrán hardware, recursos ni pediran voluntarios para trabajar en ese esfuerzo, ni permitirán que la marca CentOS se use para tal proyecto, ya que sienten que diluye lo que están tratando de hacer con el reenfoque a CentOS Stream. Dicho esto, el código es de código abierto y no intentarán evitar que nadie elija usarlo o crear sus propios paquetes a partir del código.  

***

## Conclusiones

***

La verdad que es una movida muy extraña por parte de Red Hat. Desde que fue comprado por IBM en 2018 ha estado involucrada en decisiones un tanto cuestionables y esta no es la excepción. Muchas empresas que usan CentOS porque es en esencia un RHEL además que les resulta costoso el soporte, migraron de CentOS 7 a 8 para poder tener su parque de servidores al día (además que CentOS 8 tendría soporte hasta 2029) con todo el tiempo y recursos que aquello implicó.  

CentOS fue nuestra “plataforma de prueba sensata” para MySQL, PHP, Nginx, Java y muchas otras aplicaciones. Una vez que la aplicación esté lista, podiamos implementarla en un cluster RHEL 8. Por supuesto, podemos obtener una suscripción de desarrollador de RHEL, pero solo se puede agregar una suscripción de desarrollador de Red Hat sin costo a una cuenta de usuario para tales fines.  Entonces, si en una pyme o alguna empresa cuenta con siete desarrolladores, seis desarrolladores adicionales pueden crear sus propias cuentas de usuario en developers.redhat.com, teniendo que lidiar con esas cuentas adicionales. Entonces, si los desarrolladores quieren un clon de RHEL gratuito, la siguiente mejor opción podría ser [Oracle Linux](https://www.oracle.com//linux/)  

En efecto, muchos usuarios no están contentos (incluido yo) Muy probablemente veremos una bifurcación del proyecto, lo que va a fragmentar más los esfuerzos para hacer que Linux tenga mucho mas penetración de mercado (no olvidemos lo que le pasó a [Scientific Linux](https://lwn.net/Articles/786422/)) y que esto puede significar que los proyectos mueran mucho más rápido por falta de apoyo de la comunidad.  

Solo el tiempo lo dirá...  


![Here we go](/images/centos8-ha-muerto/nixcraft.jpg "IBM: Here we go again!")  

***

## Apoya este Proyecto!!!

Si te pareció útil este artículo y el proyecto en general, considera brindarme un café :)

<style>.bmc-button img{height: 34px !important;width: 35px !important;margin-bottom: 1px !important;box-shadow: none !important;border: none !important;vertical-align: middle !important;}.bmc-button{padding: 7px 15px 7px 10px !important;line-height: 35px !important;height:51px !important;text-decoration: none !important;display:inline-flex !important;color:#ffffff !important;background-color:#5F7FFF !important;border-radius: 8px !important;border: 1px solid transparent !important;font-size: 24px !important;letter-spacing: 0.6px !important;box-shadow: 0px 1px 2px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;margin: 0 auto !important;font-family:'Cookie', cursive !important;-webkit-box-sizing: border-box !important;box-sizing: border-box !important;}.bmc-button:hover, .bmc-button:active, .bmc-button:focus {-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;text-decoration: none !important;box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;opacity: 0.85 !important;color:#ffffff !important;}</style><link href="https://fonts.googleapis.com/css?family=Cookie" rel="stylesheet"><a class="bmc-button" target="_blank" href="https://www.buymeacoffee.com/enmanuelmoreira"><img src="https://cdn.buymeacoffee.com/buttons/bmc-new-btn-logo.svg" alt="Buy me a coffee"><span style="margin-left:5px;font-size:24px !important;">Buy me a coffee</span></a>
