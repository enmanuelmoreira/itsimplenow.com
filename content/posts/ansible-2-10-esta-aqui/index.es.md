---
title: "Ansible 2.10 está aquí y estas son las Novedades"
date: 2020-09-23
lastmod: 2020-09-23
author: Enmanuel Moreira
description: "El dia 22/09/2020, se anunció la liberación de la versión 2.10 de Ansible, la cual trae muchas novedades y en este artículo te las contaré!"
draft: false

resources:
- name: "featured-image"
  src: "featured-image.png"

categories: ["Automatización"]
tags: ["infraestructura", "devops","automatización", "cloud"]

lightgallery: true
---

<!--more-->

## PROMO DigitalOcean

***

Antes de comenzar, quería contarles que hay una promoción en DigitalOcean donde te dan un crédito de USD 100.00 durante 60 días para que puedas probar los servicios que este Proveedor Cloud ofrece. Lo único que tienes que hacer es suscribirte a DigitalOcean con el siguiente enlace: <https://bit.ly/digitalocean-itsm>

***

El dia 22/09/2020, se [anunció](https://groups.google.com/g/ansible-project/c/yFnMgbjqYnU) la liberación de la versión 2.10 de Ansible, la cual trae muchas novedades y en este artículo te las contaré!

## Actualizando Ansible de 2.9.x a 2.10

***

En este momento hay un pequeño bug al actualizar Ansible desde la version 2.9.x, por lo que es recomendado primero desinstalar Ansible 2.9.x:  

```bash
$ pip uninstall ansible
```

Y luego instalar Ansible 2.10:  

```bash
$ pip install ansible==2.10 --user
```

## Cambios Importantes

***

Según el [anuncio](https://people.redhat.com/mlessard/ansiblemtl/presentations/avril2020/WhatsnewwithAnsible2.10.pdf) por parte de Red Hat  Ansible vendrá con algunos de estos cambios:  

- Todos los playbooks que se hayan escrito en Ansible 2.9.x y se ejecuten en la nueva versión 2.10, mantendrán la compatibilidad. En caso de que no funcione algún playbook, este debe ser revisado para constatar que no sea un bug del playbook.  

- Uno de los cambios importantes es que ahora algunas colecciones formaran parte de Ansible 2.10: <https://github.com/ansible-community/ansible-build-data/blob/main/2.10/CHANGELOG-v2.10.rst>  

La Lista es larga así que te recomiendo le des un vistazo: <https://github.com/ansible-community/ansible-build-data/blob/2.10.0/2.10/ansible-2.10.0.deps>  

Y aqui -> <https://github.com/ansible-collections>  

- Uno de los principales problemas que tenía Ansible es la gran diferencia de versiones entre la versión de código abierto (manejada por la comunidad y con aportes de voluntarios) y las que provenian por parte de pedidos de los clientes y/o desarrolladores de Red Hat, que tenía al menos 4500 objetos entre modulos, roles, etc; en donde podían existir dos modulos para la misma tarea (por ejemplo: Helm) Por esta razón, se decidió separar los módulos y plugins "base" y colocar los demás como Ansible Collections (roles, módulos, plugins, documentación, etc), para así organizarlos y garantizar un versionado correcto y semántico, además de que el llamado de estas colecciones sea rápida y facilmente portable. Es por ello, que al momento de la instalación de Ansible 2.10, se instalará también un paquete llamado **ansible-core** que los contendrá.  

- Por el lado Microsoft Windows, ya no serán soportados los sistemas operarivos: Windows Server 2008, 2008 R2 y Windows 7. Para más información: <https://docs.ansible.com/ansible/latest/user_guide/windows_faq.html#windows-faq-server2008>

- Está disponible la guía para portar nuestros playbooks hechos en 2.9.x a 2.10: <https://docs.ansible.com/ansible/latest/porting_guides/porting_guide_2.10.html>

![Ansible 2.10](/images/ansible-2-10/ansible-2.10.png "Cambios en Ansible 2.10")  

## Llamando una Coleccion en Ansible

Por ejemplo, en la versión 2.9.x, para poder crear una base de datos en MySQL, podíamos usar el siguente código:  

```yml
name: Create a new database with name 'bobdata'
  mysql_db:
    name: bobdata
    state: present
```

Ahora en la versión 2.10, debemos incluir la colección de las tareas que se pueden hacer con MySQL:  

```yml
- name: Create a new database with name 'bobdata'
  community.mysql.mysql_db:
    name: bobdata
    state: present
```

Donde llamamos a la coleccion community.mysql y el modulo mysql_db que permite crear la base de datos en el servidor.  

La lista de las colecciones y de como están organizadas, las podrás ver aquí: <https://docs.ansible.com/ansible/latest/collections/>  

## Ansible Fest 2020

***

Para finalizar, les recuerdo que se viene el Ansible Fest los dias 13 y 14 de Octubre de 2020, la cual será virtual y podrán registrarse aqui: <https://www.ansible.com/ansiblefest>

![Ansible Fest](/images/ansible-2-10/ansible-fest2020.png "Ansible Fest 2020")  

Espero les haya gustado este artículo, ¡hasta la próxima!

Si te pareció útil este artículo, puedes brindarme un café :)

<style>.bmc-button img{height: 34px !important;width: 35px !important;margin-bottom: 1px !important;box-shadow: none !important;border: none !important;vertical-align: middle !important;}.bmc-button{padding: 7px 15px 7px 10px !important;line-height: 35px !important;height:51px !important;text-decoration: none !important;display:inline-flex !important;color:#ffffff !important;background-color:#5F7FFF !important;border-radius: 8px !important;border: 1px solid transparent !important;font-size: 24px !important;letter-spacing: 0.6px !important;box-shadow: 0px 1px 2px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;margin: 0 auto !important;font-family:'Cookie', cursive !important;-webkit-box-sizing: border-box !important;box-sizing: border-box !important;}.bmc-button:hover, .bmc-button:active, .bmc-button:focus {-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;text-decoration: none !important;box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;opacity: 0.85 !important;color:#ffffff !important;}</style><link href="https://fonts.googleapis.com/css?family=Cookie" rel="stylesheet"><a class="bmc-button" target="_blank" href="https://www.buymeacoffee.com/enmanuelmoreira"><img src="https://cdn.buymeacoffee.com/buttons/bmc-new-btn-logo.svg" alt="Buy me a coffee"><span style="margin-left:5px;font-size:24px !important;">Buy me a coffee</span></a>
