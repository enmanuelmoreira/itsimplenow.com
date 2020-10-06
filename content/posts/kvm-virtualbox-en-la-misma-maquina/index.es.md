---
title: "KVM y VirtualBox en la misma Máquina"
date: 2020-10-06
lastmod: 2020-10-06
author: Enmanuel Moreira
description: "KVM y VirtualBox supuestamente no deberían convivir, o mejor explicado, VirtualBox no se podría ejecutar mientras los módulos de KVM estan cargados en memoria. KVM puede funcionar sin ningun problema aún teniendo VirtualBox instalado en la misma máquina. Con este panorama, te voy a explicar como usar KVM y VirtualBox en la misma máquina sin desinstalar ninguno de los dos, en vivo, sin reinicios."
draft: false

resources:
- name: "featured-image"
  src: "featured-image.png"

categories: ["Linux","Virtualización"]
tags: ["linux","vrtualización","virtualbox","kvm"]

lightgallery: true
---

<!--more-->

## PROMO DigitalOcean

***

Antes de comenzar, quería contarles que hay una promoción en DigitalOcean donde te dan un crédito de USD 100.00 durante 60 días para que puedas probar los servicios que este Proveedor Cloud ofrece. Lo único que tienes que hacer es suscribirte a DigitalOcean con el siguiente enlace: <https://bit.ly/digitalocean-itsm>

***

KVM y VirtualBox supuestamente no deberían convivir, o mejor explicado, VirtualBox no se podría ejecutar mientras los módulos de KVM estan cargados en memoria. KVM puede funcionar sin ningun problema aún teniendo VirtualBox instalado en la misma máquina.  

Con este panorama, te voy a explicar como usar KVM y VirtualBox en la misma máquina sin desinstalar ninguno de los dos, en vivo, sin reinicios.  

## Síntoma

Al ejecutar VirtualBox, podrás ver el siguiente mensaje:  

![Mensaje Error VirtualBox](/images/kvm-virtualbox/virtualbox-error.png "VirtualBox no puede iniciar una máquina Virtual")

Dice: "VirtualBox no pudo habilitar la extension AMD-V. Por favor deshabilite la extensión kernel de KVM, recompile su kernel y reinicie" Vamos a calmarnos. Este mensaje es bastante confuso, no necesitas recompilar nada. Nos tomará un par de minutos y será no destructivo para nada.  

## El Problema

VirtualBox y KVM no pueden convivir juntos. Es como una pareja que viven en la misma casa pero no se pueden ni ver. Esto quiere decir que habría que deshabilitar KVM para que VirtualBox pudiera funcionar. La cuestión es: ¿Cómo hacemos esto? La respuesta es bien sencilla: insertando y eliminando los módulos del kernel de KVM.  

Linux soporta insertar y eliminar módulos del kernel de la memoria al vuelo, sin necesidad reiniciar el sistema. Para lograr nuetsro objetivo, vamos a utilizar los comandos **insmod** y **rmmod**  

## Eliminando Módulos

Primero, vamos a ver el kernel. Ejecutamos el comando **lsmod** en búsqueda de cuales módulos están cargados en memoria y vamos a filtrar por vbox y kvm:  

```bash
sudo lsmod | grep vbox
vboxnetflt             28672  0
vboxdrv               512000  2 vboxnetadp,vboxnetflt
```

```bash
sudo lsmod | grep kvm
kvm_amd               114688  0
kvm                   823296  1 kvm_amd
```

Como se puede ver, VirtualBox usa los drivers vboxdrv y vboxnetflt, mientras que KVM usa los drivers kvm y kvm_amd (tengo una máquina con procesador AMD, si tuvieras un procesador Intel te aparecería el driver kvm_intel) Lo siguiente que vamos a hacer es deshabilitar el módulo del kernel de KVM:  

```bash
sudo rmmod kvm
sudo rmmod kvm_amd
```

Iniciamos cualquier máquina virtual y comprobamos que podemos ejecutarla.  

![VirtualBox ya funcionando](/images/kvm-virtualbox/virtualbox-funcionando.png "VirtualBox ya funcionando")

## Insertando Módulos

Así como deshabilitamos los módulos, podemos volver a habilitarlos. Necesitamos para ello ubicar los módulos en el disco y cargarlos. Se puede usar el comando **modprobe**, el cual puede ser muy útil si el módulo a cargar tuviera dependencias.  

Vamos a buscar el módulo con **find**, restringiendo la búsqueda a que solo encuentre los archivos kvm.ko y kvm-amd.ko de nuestro actual kernel [**$(uname -r)**] y lo ordenamos con **sort**:  

```bash
find /lib/modules/$(uname -r) -iname '*kvm*.ko*' | sort
/lib/modules/5.8.13-300.fc33.x86_64/kernel/arch/x86/kvm/kvm-amd.ko.xz
/lib/modules/5.8.13-300.fc33.x86_64/kernel/arch/x86/kvm/kvm-intel.ko.xz
/lib/modules/5.8.13-300.fc33.x86_64/kernel/arch/x86/kvm/kvm.ko.xz
/lib/modules/5.8.13-300.fc33.x86_64/kernel/drivers/gpu/drm/i915/gvt/kvmgt.ko.xz
/lib/modules/5.8.13-300.fc33.x86_64/kernel/drivers/ptp/ptp_kvm.ko.xz
```

Si observamos los tres primeros resultados, vemos el modulo kvm-amd, kvm-intel y kvm, vamos a cargar kvm y escogeremos dependiendo de nuestro procesador:  

```bash
sudo insmod /lib/modules/$(uname -r)/kernel/arch/x86/kvm/kvm.ko.xz
```

Para AMD:  

```bash
sudo insmod /lib/modules/$(uname -r)/kernel/arch/x86/kvm/kvm-amd.ko.xz
```

Para Intel:

```bash
sudo insmod /lib/modules/$(uname -r)/kernel/arch/x86/kvm/kvm-intel.ko.xz
```

## Haciéndolo más Simple: Scripts

Automaticemos esto de una vez. Vamos a necesitar dos scripts: Uno que deshabilite los módulos de KVM y otro que los habilite. Si quieremos ser bien estrictos, detendremos el servicio de VirtualBox cuando usemos KVM (yo en realidad no lo veo necesario) e iniciarlo después de terminar alguna máquina virtual de KVM. Comprobamos el estatus del servicio:  

```bash
sudo systemctl status vboxdrv
```

```bash
● vboxdrv.service - VirtualBox Linux kernel module
     Loaded: loaded (/usr/lib/virtualbox/vboxdrv.sh; enabled; vendor preset: disabled)
     Active: active (exited) since Tue 2020-10-06 11:09:00 -03; 6h ago
      Tasks: 0 (limit: 17813)
     Memory: 0B
     CGroup: /system.slice/vboxdrv.service
oct 06 11:08:59 localhost.localdomain systemd[1]: Starting VirtualBox Linux kernel module...
oct 06 11:08:59 localhost.localdomain vboxdrv.sh[1073]: vboxdrv.sh: Starting VirtualBox services.
oct 06 11:09:00 localhost.localdomain vboxdrv.sh[1207]: VirtualBox services started.
oct 06 11:09:00 localhost.localdomain systemd[1]: Started VirtualBox Linux kernel module.
```

Detenemos entonces el servicio:  

```bash
sudo systemctl stop vboxdrv
```


### Deshabilitando KVM y Habilitando VirtualBox

{{< admonition tip >}}
Si no sabemos muy bien donde se encuentra los programas **rmmod** e **insmod**, con which lo podemos ver:  

```bash
which rmmod
/usr/sbin/rmmod
```

```bash
which insmod
/usr/sbin/insmod
```

{{< /admonition >}}

Creamos un script para habilitar VirtualBox y deshabilitar KVM:  

```bash
vi deshabilitar_kvm.sh
```

Pega el siguiente contenido (cambia por kvm_intel en caso que tengas un procesador Intel), guardamos y salimos del editor:  

```bash
#!/bin/bash
/usr/sbin/rmmod kvm_amd
/usr/sbin/rmmod kvm
systemctl start vboxdrv
```

### Habilitando KVM y Deshabilitando VirtualBox

Haremos un script para habilitar KVM y deshabilitar VirtualBox:  

```bash
vi habilitar.sh
```

Pegamos el siguente contenido, guardamos y salimos del editor:  

```bash
#!/bin/bash
systemctl stop vboxdrv
/usr/sbin/insmod /lib/modules/$(uname -r)/kernel/arch/x86/kvm/kvm.ko.xz
/usr/sbin/insmod /lib/modules/$(uname -r)/kernel/arch/x86/kvm/kvm-amd.ko.xz
```

Les damos permisos de ejecución:  

```bash
chmod +x deshablitar_kvm.sh habilitar_kvm.sh
```

Y cuando lo ejecutamos, tiene que ser con permisos sudo:  

```bash
sudo ./deshabilitar_kvm.sh
```

```bash
sudo ./habilitar_kvm.sh
```

Bueno esto es todo por hoy, espero les haya gustado. ¡Hasta la próxima!

## Apoya este Proyecto!!!

Si te pareció útil este artículo y el proyecto en general, considera brindarme un café :)

<style>.bmc-button img{height: 34px !important;width: 35px !important;margin-bottom: 1px !important;box-shadow: none !important;border: none !important;vertical-align: middle !important;}.bmc-button{padding: 7px 15px 7px 10px !important;line-height: 35px !important;height:51px !important;text-decoration: none !important;display:inline-flex !important;color:#ffffff !important;background-color:#5F7FFF !important;border-radius: 8px !important;border: 1px solid transparent !important;font-size: 24px !important;letter-spacing: 0.6px !important;box-shadow: 0px 1px 2px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;margin: 0 auto !important;font-family:'Cookie', cursive !important;-webkit-box-sizing: border-box !important;box-sizing: border-box !important;}.bmc-button:hover, .bmc-button:active, .bmc-button:focus {-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;text-decoration: none !important;box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;opacity: 0.85 !important;color:#ffffff !important;}</style><link href="https://fonts.googleapis.com/css?family=Cookie" rel="stylesheet"><a class="bmc-button" target="_blank" href="https://www.buymeacoffee.com/enmanuelmoreira"><img src="https://cdn.buymeacoffee.com/buttons/bmc-new-btn-logo.svg" alt="Buy me a coffee"><span style="margin-left:5px;font-size:24px !important;">Buy me a coffee</span></a>
