---
title: "Que es Infraestructura como Código (IaC) y PORQUÉ deberias APRENDERLO"
date: 2020-09-01
lastmod: 2020-09-01
author: Enmanuel Moreira
description: "En el mundo actual, donde pasamos desde la máquina física, pasando de la máquina virtual a los contenedores, se ha hecho necesario desplegar con la mayor rapidez y agilidad posible, recursos que permitan poner en linea un complejo sistema web de un e-commerce o algún ERP."
draft: false

resources:
- name: "featured-image"
  src: "featured-image.jpg"

categories: ["Infraestructura"]
tags: ["infraestructura", "devops","codigo", "cloud"]

lightgallery: true
---

<!--more-->

## PROMO DigitalOcean

***

Antes de comenzar, quería contarles que hay una promoción en DigitalOcean donde te dan un crédito de USD 100.00 durante 60 días para que puedas probar los servicios que este Proveedor Cloud ofrece. Lo único que tienes que hacer es suscribirte a DigitalOcean con el siguiente enlace: <https://bit.ly/digitalocean-itsm>

***

## Introducción

***

En el mundo actual, donde pasamos desde la máquina física, pasando de la máquina virtual a los contenedores, se ha hecho necesario desplegar con la mayor rapidez y agilidad posible, recursos que permitan poner en linea un complejo sistema web de un e-commerce o algún ERP.  

Para hacerlo lo mas rápido y simple posible, necesitamos de herramientas que permitan **AUTOMATIZAR** cada uno de los procesos de: escoger el servidor, particionar los discos, configurar el sistema operativo, levantar los servicios necesarios, aplicar reglas de firewall, entre muchos otros. Si bien la Infraestructura como Servicio (IaaS) soluciona parte de este problema, las tareas repetitivas de crear manualmente los servidores necesarios, hace la tarea menos eficiente. Afortunadamente los proveedores Cloud proporcionan una API o una interfaz donde, con el envío de comandos, se puede crear esta infraestructura de manera mucho mas ágil.  

Por ello, se implanta un nuevo concepto de Infraestructura como Código, el cual vamos a desarrollar en este articulo.  

## ¿Qué es Infraestructura como Código?

***

Infraestructura como Código (IaC) es la práctica de definir y aprovisionar recursos informáticos (bien sea hardware o software) de una manera automatizada, a través de un formato que pueda ser leído por la máquina y facilmente entendido por quien lo escribe.  

A pesar que IaC es a menudo usado en ambientes Cloud, no necesariamente hay que usar CLoud para desplegar IaC. A medida que el sistema de gestión de la infraestructura física lo permita, tambien se podría desplegar IaaC.  

Existen diversas herramientas para logar este propósito, algunas propietarias como CloudFormation de AWS, Resource Manager de Microsoft Azure, Oracle Resource Manager, Heat de OpenStack, solo funcionan en sus respectivos productos, obligandote a "casarte" con el proveedor de Cloud. Sin embargo, hay una herramenta que no importa cual proveedor escojas (siempre que lo soporte), es el caso de **[Terraform](https://www.terraform.io)** de la empresa **[HashiCorp](https://www.hashicorp.com/)**.  

## Objetivos de IaC

***

- Automatizar los procesos manuales al máximo posible.  
  
- Eliminar los límites entre las aplicaicones y los entornos en los que se ejecutan.  

- Establecer un flujo de trabajo flexible que facilite la colaboración de todos los implicados en el proceso de desarrollo en toda la empresa.  

- Presentar los cambios y movimientos de contenido de forma transparente y detallada en todo momento.  

- Lograr que la configuración del hardware sea tan verificable como la del software.  

## Valor Añadido y Ventajas

***

El valor de IaC puede englobarse en tres categorias medibles: costo (reducción), velocidad (ejecución más rápida) y riesgo (eliminación de errores y violaciones de seguridad)

La reducción de costos tiene como objetivo no solo ayudar a la empresa financieramente, sino tambien en términos de personas y esfuerzo realizado, lo que significa que al eliminar el componente manual, las personas son capaces de encocar esfuerzos en otras áreas en común.  

La automatización de la infraestructura permite aumentar la velocidad con la que se configura y tiene como objetivo proporcionar visibilidad para auydar a los equipos de la empresa a trabajar de forma más rápida y eficiente, eliminando, de esta manera, el riesgo asociado a los errores humanos, como la mala configuración manual; disminuyendo el tiempo de inactividad y aumentando la confibilidad.  

## Entornos Propicios para la Automatización

***

Ya no basta con tener interfaces y **[API](https://es.wikipedia.org/wiki/Interfaz_de_programaci%C3%B3n_de_aplicaciones)** para poder programar la infraestructura. Queremos gestionarla de forma sencilla. Y, para ello, qué mejor que ofrecer a los informáticos para que gestionen su infraestructura la programación, que es un recurso que ya conocen. Así surge el paradigma *Infraestructure as Code* (IaC) y las herramientas de automatización de configuración continua o *Continuous Configuration Automation* (CCA). En el mundo TI ya son muy conocidas herramientas como **[Ansible](https://ansible.com/)** así como diferentes plataformas de gestión en la nube *o* CMP. Pero esta automatización no es exclusiva del mundo TI, también ha llegado con fuerza a los entornos de telecomunicaciones de la mano de la virtualización de la red y **[NFV](https://es.wikipedia.org/wiki/Virtualizaci%C3%B3n_de_funciones_de_red)**. Tecnologías como Open MANO, ONAP o TOSCA aparecen para modelar y automatizar los despliegues de servicios de red e infraestructuras.  

Cualquier entorno de infraestructuras es susceptible de someterse a las mejores prácticas de la IaC y la automatización. No obstante, algunos son más propicios que otros. En mi opinión, tres factores fundamentales caracterizan a los entornos más propicios:  

- **Gran volumen de infraestructura**. Para que automatizar algo no sea un esfuerzo inútil, tiene que haber tareas repetitivas, muchos elementos similares que gestionar.  
  
- **Alto ritmo de innovación**. En entornos donde la innovación es muy alta, la probabilidad de que se ejecuten despliegues “a mano” no controlados por parte de administradores o usuarios avanzados, es elevada. La IaC ayuda a sacarlo a la luz y tener orden.  
  
- **Necesidad de estandarización**. Al contrario que en Telecomunicaciones, en TI no existen organismos internacionales que estandaricen el uso de las tecnologías. Esto obliga a cada organización a crearse sus propios estándares, normativas y arquitecturas de referencia. Tradicionalmente esta normativa se implementaba en documentos que, con los años, acababan desactualizados en algún rincón perdido de la Intranet de usuarios. La IaC convierte ese documento que nadie leía en un proceso tecnológico que gobierna la infraestructura y asegura su cumplimiento.  

Dado que ni un entorno *mainframe* ni un sistema UNIX legado cumplen los requisitos anteriores no son adecuados para obtener los beneficios de IaC. Una red de telecomunicaciones, por ejemplo, reúne las dos primeras condiciones pero, al tener estándares y arquitecturas muy claras y regladas, hay mucho más orden que en la TI y los procesos de creación de red están muy consolidados. Los entornos de *business intelligence* y *big data* son los más idóneos, pues reúnen las tres características, por ello la IaC suele aparecer de forma casi espontánea desde el inicio de estos proyectos.  

![Principal](/images/por-que-aprender-iac/imagen1.png)

## ¿Por qué deberías estar aprendiendo Infraestructura como Código?

***

La Infraestructura como Código es hoy en día, una parte crucial de la filosofía DevOps. La computación en la nube es apenas la punta del iceberg, IaC lleva a la computación en la nube a su máximo potencial, y eso libera tanto a Administradores de Sistemas como a Desarrolladores a realizar tareas manuales sujetas a errores humanos, lo que impacta significativamente en los costos y provee eficiencia en todas las etapas del proceso de desarrollo de software.  

Como Adminstrador de Sistemas, aprender Infraestructura como Código te permitirá, además de avanzar con tu carrera profesional, te brindará una serie de habilidades en las cuales podrás agilizar y ser mas productivo en tareas de alto valor, eliminar aquellos procesos repetivivos y enfocar la atención en otras áreas.  

Te recomiendo comiences hoy mismo a aprender IaC y automatizar tu propio laboratorio casero :)  

Espero les haya gustado este artículo, ¡hasta la próxima!

## Apoya este Proyecto!!!

Si te pareció útil este artículo y el proyecto en general, considera brindarme un café :)

<style>.bmc-button img{height: 34px !important;width: 35px !important;margin-bottom: 1px !important;box-shadow: none !important;border: none !important;vertical-align: middle !important;}.bmc-button{padding: 7px 15px 7px 10px !important;line-height: 35px !important;height:51px !important;text-decoration: none !important;display:inline-flex !important;color:#ffffff !important;background-color:#5F7FFF !important;border-radius: 8px !important;border: 1px solid transparent !important;font-size: 24px !important;letter-spacing: 0.6px !important;box-shadow: 0px 1px 2px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;margin: 0 auto !important;font-family:'Cookie', cursive !important;-webkit-box-sizing: border-box !important;box-sizing: border-box !important;}.bmc-button:hover, .bmc-button:active, .bmc-button:focus {-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;text-decoration: none !important;box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;opacity: 0.85 !important;color:#ffffff !important;}</style><link href="https://fonts.googleapis.com/css?family=Cookie" rel="stylesheet"><a class="bmc-button" target="_blank" href="https://www.buymeacoffee.com/enmanuelmoreira"><img src="https://cdn.buymeacoffee.com/buttons/bmc-new-btn-logo.svg" alt="Buy me a coffee"><span style="margin-left:5px;font-size:24px !important;">Buy me a coffee</span></a>
