---
title: "Como Instalar Foreman en CentOS 8 / RHEL 8"
date: 2020-08-25
lastmod: 2020-08-25
author: Enmanuel Moreira
description: "Foreman es una herramienta de código abierto que permite el aprovisionamiento automatizado y la configuración de sistemas Linux. Puede integrarse con diversas herramientas de automatización como Ansible, Chef, Salt y Puppet, que están disponibles como plugins y proveer una gestión del ciclo de vida de una infraestructura completa. En este post les voy a explicar como instalar Foreman en un sistema CentOS 8 / RHEL 8"
draft: false

resources:
- name: "featured-image"
  src: "featured-image.jpg"

categories: ["Linux","Automatización","Tutorial"]
tags: ["redhat","centos","automatización","infraestructura"]

lightgallery: true
---

<!--more-->

## PROMO DigitalOcean

***

Antes de comenzar, quería contarles que hay una promoción en DigitalOcean donde te dan un crédito de USD 100.00 durante 60 días para que puedas probar los servicios que este Proveedor Cloud ofrece. Lo único que tienes que hacer es suscribirte a DigitalOcean con el siguiente enlace: <https://bit.ly/digitalocean-itsm>

***

## Introducción

***

[Foreman](https://www.theforeman.org/) es una herramienta de código abierto que permite el aprovisionamiento automatizado y la configuración de sistemas Linux. Puede integrarse con diversas herramientas de automatización como Ansible, Chef, Salt y Puppet, que están disponibles como plugins y proveer una gestión del ciclo de vida de una infraestructura completa. En este post les voy a explicar como instalar Foreman en un sistema CentOS 8 / RHEL 8.  

## Arquitectura de Foreman

***

Un servidor configurado con Foreman siempre contendrá una instancia central la cual es responsable de proveer una interfaz web, configuraciones de los nodos, archivos de configuración iniciales de los hosts, etc. Sin embargo, si el servidor instalado soporta instalaciones desatendidas entonces las otras operaciones necesitan ser realizadas para automatizar completamente el proceso. El Smart Proxy gestiona los servicios remotos y es generalmente instalado con todos los componentes de Foreman para administrar TFTP, DHCP, DNS, Puppet, Puppet CA, Ansible, Salt, y Chef.

## Smart-Proxy

***

El Smart-Proxy está localizado dentro o cerca de una máquina que realiza una función y ayuda a Foreman a orquestar el proceso de configurar un nuevo host. Colocar el proxy dentro o cerca del actual servicio también ayuda a reducir la latencia en una infraestructura distribuida de gran tamaño.  

![Arquitectura de Foreman](/images/foreman-installation/foreman_architecture.png "Arquitectura de Foreman")

## Características de Foreman

***

Las principales características de Foreman, son las siguientes:  

- Descubre, aprovisiona y actualiza una infraestructura física completa.  
- Crea y administra instancias en un ambiente de virtualización tanto en nube privada como pública.  
- Instala sistemas operativos a través de, medios locales o desde templates o imágenes.  
- Controla y provee reportes desde el software de manejo de configuración.  
- Agrupa los hosts y los administra en serie, independientemente de la ubicación donde se encuentren.  
- Se puede ver el histórico de cambios para efectos de auditoría y troubleshooting.  
- Interfaz gráfica de usuario, JSON REST API y CLI para Linux.  
- Puede extenderse las funcionalidad si es necesario, a través de una arquitectura robusta de plugins.  

Después de instalar Foreman, la tarea principal a ejecutar es la de instalar correctamente los plugins que formarán parte de la Infraestructura. Si se usa RHEV como Infraestructura de virtualización, se necesita especificar los plugins que serán habilitados para aprovisionar y eliminar recursos. El mismo modelo aplica para todos los demás softwares de gestión de Infraestructura como KVM, Xen, CloudStack, OpenStack y otros.  

## Instalando Foreman

***

### Configurando el nombre del host

Es necesario configurar un nombre de host para el servidor donde va a ser instalado. Voy a utilizar el dominio itsimplenow.com, por lo que debes cambiarlo por el de tu Infraestructura actual.  

```bash
[centos@foreman ~]$ sudo hostnamectl set-hostname foreman.itsimplenow.com
```

Con su editor de textos de confianza, modificamos el archivo /etc/hosts para colocar la dirección IP y el nombre FQDN del servidor:  

```bash
192.168.0.10 foreman.itsimplenow.com foreman
```

Comprobamos si está tomando la dirección IP y que no sea la de localhost (127.0.0.1):  

```bash
[centos@foreman ~]$ ping -c 2 $(hostname -f)
PING foreman.itsimplenow.com (192.168.0.10) 56(84) bytes of data.
64 bytes from foreman.itsimplenow.com (192.168.0.10): icmp_seq=1 ttl=64 time=0.052 ms
64 bytes from foreman.itsimplenow.com (192.168.0.10): icmp_seq=2 ttl=64 time=0.035 ms

--- foreman.itsimplenow.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1023ms
rtt min/avg/max/mdev = 0.035/0.043/0.052/0.010 ms
```

Si ping da como resultado 127.0.0.1, modifica o elimina la entrada en el archivo /etc/hosts en caso de estar presente.  

El nombre de host configurado puede ser confimado con el comando:  

```bash
[centos@foreman ~]$ host foreman.itsimplenow.com
```

### Instalar y Configurar NTP para Sincronizar la Fecha y Hora

***

Chronyd es la alternativa de sincronización de tiempo para sistemas CentOS / RHEL, la instalamos con el siguiente comando:  

```bash
[centos@foreman ~]$ sudo dnf install chrony -y
```

Una vez instalado, configuramos nuestra zona horaria:  

```bash
[centos@foreman ~]$ sudo timedatectl set-timezone America/Santiago
```

Iniciamos chronyd y lo habilitamos para que se levante al iniciar el sistema:  

```bash
[centos@foreman ~]$ sudo systemctl enable --now chronyd
```

Sincronizamos la fecha y hora:  

```bash
[centos@foreman ~]$ sudo chronyc sources
210 Number of sources = 5
MS Name/IP address         Stratum Poll Reach LastRx Last sample
===============================================================================
#* PHC0                          0   3   377     8  -1479ns[  +11us] +/- 1358ns
^- 13.86.101.172                 3   6   377    77  +2545us[+2410us] +/-   41ms
^- 44.190.6.254                  2   6   377    13   -678us[ -679us] +/-   57ms
^- ns2.nuso.cloud                2   6   377    11  -1875us[-1870us] +/-   46ms
^- linode.ibendit.com            3   6   367    13  +2223us[+2222us] +/-   93ms
```

Y ahora confirmamos:  

```bash
[centos@foreman ~]$ timedatectl
               Local time: Tue 2020-09-15 16:50:41 -03
           Universal time: Tue 2020-09-15 19:50:41 UTC
                 RTC time: Tue 2020-09-15 19:50:41
                Time zone: America/Santiago (-03, -0300)
System clock synchronized: yes
              NTP service: n/a
          RTC in local TZ: no
```

### Añadiendo los repositorios de Foreman y Puppet

***

#### Puppet

```bash
[centos@foreman ~]$ sudo dnf -y install https://yum.puppet.com/puppet6-release-el-8.noarch.rpm
```

#### Foreman

```bash
[centos@foreman ~]$ sudo dnf -y install https://yum.theforeman.org/releases/2.1/el8/x86_64/foreman-release.rpm
```

### Añadiendo el Instalador de Foreman

***

Una vez que ambos repositorios estén instalados, instalamos el instalador (valga la redundancia) de Foreman:  

```bash
[centos@foreman ~]$ sudo dnf -y install foreman-installer
```

Cuando la instalación termine, configuraremos los componentes de Foreman. El instalador no es mas que modulos de Puppet que instala y configura todo por nosotros, lo siguiente:  

- Servidor Apache HTTP con SSL (usando un certificado Puppet autofirmado)
- Ejecución de Foreman bajo mod_passenger.
- Configura el Smart Proxy para Puppet, TFTP y SSL.
- Ejecución del Puppet master bajo mod_passenger
- Configuración del agente Puppet.
- Servidor TFTP (bajo xinetd en sistemas Red Hat)

La instalación de foreman se ejecuta en un modo no interactivo por defecto, pero la configuración puede ser personalizada proveyendo algunas de las siguientes opciones:  

```bash
[centos@foreman ~]$ foreman-installer --help
```

Para que el instalador se ejecute en modo interactivo:  

```bash
[centos@foreman ~]$ sudo foreman-installer -i
```

En caso de querer instalalo con el modo no interactivo:  

```bash
[centos@foreman ~]$ sudo foreman-installer
Preparing installation Done
Executing: foreman-rake upgrade:run
foreman-rake upgrade:run finished successfully!
  Success!
  * Foreman is running at https://foreman.itsimplenow.com
      Initial credentials are admin / 7o6gqYujGYZsofj4
  * Foreman Proxy is running at https://foreman.itsimplenow.com:8443
  The full log is at /var/log/foreman-installer/foreman.log
```

Veremos la credencial de acceso admin (Initial credentials are admin / 7o6gqYujGYZsofj4) y la url (<https://foreman.itsimplenow.com>)

### Configurando el Firewall

***

Abrimos los puertos requeridos:  

```bash
[centos@foreman ~]$ sudo firewall-cmd --add-port={22,53,80,443,3000,8140,3306,5432,8443,5910-5930}/tcp --permanent
```

```bash
[centos@foreman ~]$ sudo firewall-cmd --add-port=67-69/udp --permanent
```

Recargamos el firewall:  

```bash
[centos@foreman ~]$ sudo firewall-cmd --reload
```

Y ya lo tendremos instalado.  

![Foreman Login](/images/foreman-installation/foreman_login.png "Inicio de Sesión")

![Foreman Dashboard](/images/foreman-installation/foreman_dashboard.png "Dashboard")

Espero les haya gustado este tutorial, ¡hasta la próxima!

## Apoya este Proyecto!!!

Si te pareció útil este artículo y el proyecto en general, considera brindarme un café :)

<style>.bmc-button img{height: 34px !important;width: 35px !important;margin-bottom: 1px !important;box-shadow: none !important;border: none !important;vertical-align: middle !important;}.bmc-button{padding: 7px 15px 7px 10px !important;line-height: 35px !important;height:51px !important;text-decoration: none !important;display:inline-flex !important;color:#ffffff !important;background-color:#5F7FFF !important;border-radius: 8px !important;border: 1px solid transparent !important;font-size: 24px !important;letter-spacing: 0.6px !important;box-shadow: 0px 1px 2px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;margin: 0 auto !important;font-family:'Cookie', cursive !important;-webkit-box-sizing: border-box !important;box-sizing: border-box !important;}.bmc-button:hover, .bmc-button:active, .bmc-button:focus {-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;text-decoration: none !important;box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;opacity: 0.85 !important;color:#ffffff !important;}</style><link href="https://fonts.googleapis.com/css?family=Cookie" rel="stylesheet"><a class="bmc-button" target="_blank" href="https://www.buymeacoffee.com/enmanuelmoreira"><img src="https://cdn.buymeacoffee.com/buttons/bmc-new-btn-logo.svg" alt="Buy me a coffee"><span style="margin-left:5px;font-size:24px !important;">Buy me a coffee</span></a>
