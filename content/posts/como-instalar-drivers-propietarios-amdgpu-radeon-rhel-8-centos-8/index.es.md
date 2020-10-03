---
title: "Como Instalar los Drivers Propietarios AMDGPU Radeon en RHEL 8 / Centos 8"
date: 2020-10-01
lastmod: 2020-10-01
author: Enmanuel Moreira
description: "Para nadie es un secreto el calvario que pasamos los usuarios de Linux con las tarjetas gráficas de AMD, por el poco soporte (a.k.a interés) de las grandes empresas de productos gráficos de hacer drivers decentes y brindar soporte para este sistema operativo. Tuve algunos problemas para instalar mi editor de videos favorito DaVinci Resolve en CentOS (al parecer el driver de código abierto no proporcionaba el soporte a OpenCL) lo que me obligó a investigar el porqué. AMD soporta pocas distribuciones, entre las cuales están Ubuntu y RHEL, por lo que en el siguiente artículo te voy a mostrar como instalar los drivers privativos de AMDGPU en CentOS 8."
draft: false

resources:
- name: "featured-image"
  src: "featured-image.jpg"

categories: ["Linux"]
tags: ["tutorial"]

lightgallery: true
---

<!--more-->

## PROMO DigitalOcean

***

Antes de comenzar, quería contarles que hay una promoción en DigitalOcean donde te dan un crédito de USD 100.00 durante 60 días para que puedas probar los servicios que este Proveedor Cloud ofrece. Lo único que tienes que hacer es suscribirte a DigitalOcean con el siguiente enlace: <https://bit.ly/digitalocean-itsm>

***

Para nadie es un secreto el calvario que pasamos los usuarios de Linux con las tarjetas gráficas de AMD, por el poco soporte (a.k.a interés) de las grandes empresas de productos gráficos de hacer drivers decentes y brindar soporte para este sistema operativo. Tuve algunos problemas para instalar mi editor de videos favorito DaVinci Resolve en CentOS que no reconocia el componente OpenCL, lo que me obligó a investigar el porqué.  


El software Radeon de AMD soporta pocas distribuciones, entre las cuales están Ubuntu y RHEL, por lo que en el siguiente artículo te voy a mostrar como instalar los drivers privativos de AMDGPU en CentOS 8.  

## Tarjetas Gráficas Soportadas

***

Según la página de [AMD](https://www.amd.com/en/support/kb/release-notes/rn-amdgpu-unified-linux-20-20), están soportadas las siguientes tarjetas:  

- AMD Radeon™ RX 5700/5600/5500 Series Graphics

- AMD Radeon™ Pro WX-series​

- AMD Radeon™ VII Series Graphics​

- AMD Radeon™ Pro WX 9100

- AMD Radeon™ RX Vega Series Graphics​

- AMD Radeon™ Pro WX 8200

- AMD Radeon™ Vega Frontier Edition

- ​AMD FirePro™ W9100

- AMD Radeon™ RX 550/560/570/580/590 Series Graphics

- AMD FirePro™ W8100

- AMD Radeon™ RX 460/470/480 Graphics

- AMD FirePro™ W7100

- AMD Radeon™ Pro Duo

- AMD FirePro™ W5100

- AMD Radeon™ R9 Fury/Fury X/Nano Graphics

- AMD FirePro™ W4300

- AMD Radeon™ R9 380/380X/390/390X Graphics

- AMD Radeon™ R9 285/290/290X Graphics 

- AMD Radeon™ R9 360 Graphics

## Descargando el Driver Radeon

***

Descargamos el driver desde la página:  

<https://www.amd.com/en/support/kb/release-notes/rn-amdgpu-unified-linux-20-20>:  

```bash
wget https://drivers.amd.com/drivers/linux/amdgpu-pro-20.20-1089974-rhel-8.2.tar.xz
```

Lo desempaquetamos:  


```bash
tar xfJv https://drivers.amd.com/drivers/linux/amdgpu-pro-20.20-1089974-rhel-8.2.tar.xz
```

Y le cambiamos el nombre a la carpeta a **amdgpu**

```bash
mv amdgpu-pro-20.20-1089974-rhel-8.2 amdgpu
```

## Creando el Repositorio Local

***

El instalador va a instalar (obvio) todos los paquetes necesarios para que los drivers funcionen. Vamos a crear un repositorio local el cual contendrá la carpeta donde se encuentran los archivos rpm.  

Abrimos el editor de textos de su preferencia (yo usaré vim):  

```bash
sudo vim /etc/yum.repos.d/amdgpu.repo
```

y pegamos el siguiente contenido:  

```bash
[amdgpu]
name=AMDGPU Packages
baseurl=file:///var/local/amdgpu/
enabled=1
skip_if_unavailable=1
gpgcheck=0
cost=500
metadata_expire=300
```

Donde **baseurl=file:///var/local/amdgpu/** lo vamos a reemplazar por el directorio donde se encuentre ubicada la carpeta **amdgpu**.    


## Instalando el Driver Radeon

***

Entramos a la carpeta **amdgpu**:  

```bash
cd amdgpu
```

Ejecutamos el script de instalación:  

```bash
sudo ./amdgpu-install
```

Va a tardar unos minutos en instalar y compilar el driver, una vez instalado, reiniciamos el sistema:  

```bash
sudo systemctl reboot
```

Ya reiniciado el sistema, procedemos a instalar los componentes de OpenCL:

- Si tenemos una tarjeta gráfica POLARIS o anterior (la mia es una RX560X):

```bash
sudo dnf install ibdrm-amdgpu libdrm-amdgpu-common clinfo-amdgpu-pro opencl-amdgpu-pro-comgr amdgpu-pro-core opencl-orca-amdgpu-pro-icd libopencl-amdgpu-pro
```

- Si nuestra gráfica es VEGA o superior:  

```bash
sudo dnf install libdrm-amdgpu libdrm-amdgpu-common clinfo-amdgpu-pro opencl-amdgpu-pro-comgr amdgpu-pro-core opencl-amdgpu-pro-icd libopencl-amdgpu-pro
```

Ya con esos pasos, tendremos instalados los drivers.  

Espero les haya gustado este artículo, ¡hasta la próxima!

## Referencias

***

- [Radeon™ Software for Linux® installation instructions](https://amdgpu-install.readthedocs.io/en/latest/)

***

Si te pareció útil este artículo, puedes brindarme un café :)

<style>.bmc-button img{height: 34px !important;width: 35px !important;margin-bottom: 1px !important;box-shadow: none !important;border: none !important;vertical-align: middle !important;}.bmc-button{padding: 7px 15px 7px 10px !important;line-height: 35px !important;height:51px !important;text-decoration: none !important;display:inline-flex !important;color:#ffffff !important;background-color:#5F7FFF !important;border-radius: 8px !important;border: 1px solid transparent !important;font-size: 24px !important;letter-spacing: 0.6px !important;box-shadow: 0px 1px 2px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;margin: 0 auto !important;font-family:'Cookie', cursive !important;-webkit-box-sizing: border-box !important;box-sizing: border-box !important;}.bmc-button:hover, .bmc-button:active, .bmc-button:focus {-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;text-decoration: none !important;box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;opacity: 0.85 !important;color:#ffffff !important;}</style><link href="https://fonts.googleapis.com/css?family=Cookie" rel="stylesheet"><a class="bmc-button" target="_blank" href="https://www.buymeacoffee.com/enmanuelmoreira"><img src="https://cdn.buymeacoffee.com/buttons/bmc-new-btn-logo.svg" alt="Buy me a coffee"><span style="margin-left:5px;font-size:24px !important;">Buy me a coffee</span></a>
