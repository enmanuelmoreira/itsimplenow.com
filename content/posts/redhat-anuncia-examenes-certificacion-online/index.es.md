---
title: "Red Hat anuncia sus exámenes de Certificación para ser presentados Online"
date: 2020-08-10
lastmod: 2020-08-10
author: Enmanuel Moreira
description: "Hay buenas noticias para todos aquellos que quedaron sin poder presentar los examenes de Red Hat (recordemos que los examenes de certificación de Red Hat solo se pueden hacer en un Centro Autorizado), y viendo la actual situación mundial con el COVID-19, finalmente Red Hat anunció la disponibilidad de algunos sus examenes para ser presentados en casa/oficina."
draft: false

resources:
- name: "featured-image"
  src: "featured-image.png"

categories: ["Certificaciones"]
tags: ["redhat","certificaciones","sysadmin","rhcsa","rhce","rhel"]

lightgallery: true
---

<!--more-->

¡Hola a todos!

Hay buenas noticias para todos aquellos que quedaron sin poder presentar los examenes de Red Hat (recordemos que los examenes de certificación de Red Hat solo se pueden hacer en un Centro Autorizado), y viendo la actual situación mundial con el COVID-19, finalmente Red Hat [anunció](https://www.redhat.com/en/about/press-releases/red-hat-launches-remote-certification-exams), la disponibilidad de algunos sus examenes para ser presentados en casa/oficina.

## Exámenes Disponibles  

***

A partir del 3 de agosto de 2020, estarán disponibles los siguientes examenes para realizarlos en linea:

- [Red Hat Certified System Administrator (RHCSA) exam (EX200V8K)](https://www.redhat.com/en/services/training/ex200-red-hat-certified-system-administrator-rhcsa-exam)

- [Red Hat Certified Engineer (RHCE) exam (EX294V8K)](https://www.redhat.com/en/services/training/ex294-red-hat-certified-engineer-rhce-exam-red-hat-enterprise-linux-8)

- [Red Hat Certified Specialist in OpenShift Administration exam (EX280V42K)](https://www.redhat.com/en/services/training/ex280-red-hat-certified-specialist-in-openshift-administration-exam)

- [Red Hat Certified Specialist in OpenShift Application Development exam (EX288V42K)](https://www.redhat.com/en/services/training/ex288-red-hat-certified-specialist-openshift-application-development-exam)

## Requerimientos Técnicos

***

Según el documento [Getting Ready for your Red Hat Remote Exam](https://learn.redhat.com/jfvwy86652/attachments/jfvwy86652/certification_resources/2/3/Getting%20Ready%20for%20your%20Red%20Hat%20Remote%20Exam_4_Aug_20.pdf), se necesitarán cumplir con las siguientes condiciones:  

- Una PC con un solo monitor activo, o una Laptop con procesador Intel/AMD. En el caso de la Laptop. Se permitirá conectar un monitor externo, siempre y cuando la pantalla de la laptop se cierre al momento de hacer el examen.

- 4GB de RAM como mínimo.

- Unidad USB de 8GB mínimo de almacenamiento.
{{< admonition warning >}}
La Unidad de USB necesita ser borrada completamente para crear el ambiente del examen, por lo que se recomienda hacer un respaldo de los datos contenidos en el.
{{< /admonition >}}

- Una Webcam externa que tenga al menos 1 m. de cable.

- Se permite un USB Hub para conectar mouse, teclado, sonido y micrófono.

- Mouse y Teclado. Pueden ser externos siempre y cuando sen cableados (no se permiten mouse o teclados inalámbricos) y si se usa una laptop, esta debe estar cerrada al momento de realizar el examen.

- Sonido y micrófono. Se debe verificar que el micrófono no esté en mute al momento de realizar el examen.

- Conexión a Internet con velocidades mínimas de descarga de 768Kbps y subida de 512Kbps.

- Conexión a la Red cableado. No se permite conexión por Wi-Fi para asegurar la calidad de la transmisión durante el examen.
  
- Si se realiza el examen en una Laptop, la batería debe estar lo suficientemente cargada y estar conectada al cargador al momento de realizar el examen.

## Configuración del ambiente del examen

***

Para configurar el ambiente del examen, necesitamos tres cosas:  

- Una Unidad USB de 8 GB de capacidad mínimo.  
- El programa Fedora Image Writer.  
- La imagen .iso que contiene el ambiente de la prueba.  

{{< admonition tip >}}
Se requiere ser Administrador del sistema para poder ejecutar los pasos siguientes. Obten permisos de Administrador de su departamento de IT si estás usando un computador provisto por la empresa.
{{< /admonition >}}

### Windows 7, 8.1 / 10

1. Descargamos la imagen .iso del ambiente del examen desde [aquí](https://static.redhat.com/downloads/training-certification/rhrexboot.iso)

2. Conectamos la unidad USB al computador.

3. Descargamos e instalamos el Fedora Media Writer desde [aquí](https://getfedora.org/fmw/FedoraMediaWriter-win32-latest.exe)

![Licencia](/images/redhat-online-exams/fedora-media-writer-0.png "Licencia del Programa de Instalación")

![Ubicacion](/images/redhat-online-exams/fedora-media-writer-1.png "Ubicación de la Instalación")

![Instalación Completa](/images/redhat-online-exams/fedora-media-writer-2.png "Instalación Completa")

![Final](/images/redhat-online-exams/fedora-media-writer-3.png "Finalizando Instalador")

4. Abrimos Fedora Media Writer para escribir la imagen .iso del ambiente del examen:  
a. Seleccionamos **Imagen Personalizada**  
![FIW](/images/redhat-online-exams/fedora-media-writer-4.png "Imagen Personalizada")
b. Escogemos la imagen .iso donde la hayamos descargado luego damos click a Abrir y nos llevará a la pantalla de Escribir Imagen Personalizada.  
![FIW](/images/redhat-online-exams/fedora-media-writer-5.png "Seleccionando Imagen")
c. Si la unidad USB está conectada al computador, Fedora Media Writer mostrará la unidad para crear el ambiente del examen  
{{< admonition warning >}}
Fedora Media Writes borrará la unidad USB para crear el ambiente del examen. Haz un respaldo de todos los datos antes de continuar.  
{{< /admonition >}}
![FIW](/images/redhat-online-exams/fedora-media-writer-6.png "Seleccionando Imagen")  
d. Damos click en Escribir al disco par iniciar el proceso de escritura.  
![FIW](/images/redhat-online-exams/fedora-media-writer-7.png "Escribiendo imagen")
![FIW](/images/redhat-online-exams/fedora-media-writer-8.png "Comprobando datos")
e. Finalizamos Fedora Media Writer  
![FIW](/images/redhat-online-exams/fedora-media-writer-9.png "Finalizando")

5. Ahora iniciamos el disco USB con el ambiente del examen  

### Fedora 30 - Fedora 32

{{< admonition info >}}
Se necesitan privilegios de root o sudo para poder ejecutar las siguientes operaciones.  
{{< /admonition >}}

#### Método 1: Usando Fedora Media Writer

1. Descargamos la imagen .iso del ambiente del examen desde [aquí](https://static.redhat.com/downloads/training-certification/rhrexboot.iso)
2. Instalamos Fedora Image Writer desde Software:  
![Software](/images/redhat-online-exams/fedora-0.png "Aplicación Software")
![Software](/images/redhat-online-exams/fedora-1.png "Buscamos el programa Fedora Media Writer")
![Instalación](/images/redhat-online-exams/fedora-2.png "Instalamos Fedora Media Writer")
![Fedora Media Writer](/images/redhat-online-exams/fedora-3.png "Ejecutamos Fedora Media Writer")
3. Escogemos la imagen .iso donde la hayamos descargado luego damos click a Abrir y nos llevará a la pantalla de Escribir Imagen Personalizada.  
![Imagen](/images/redhat-online-exams/fedora-4.png "Imagen Personalizada")
![Chat Support](/images/redhat-online-exams/fedora-5.png "Seleccionando Imagen")
4. Iniciamos la escritura del disco USB, debemos proporcionar nuestra clave de root.  
![Chat Support](/images/redhat-online-exams/fedora-6.png "Escribiendo al disco")
5. Una vez finalizado, iniciamos el disco USB con el ambiente del examen.  

#### Método 2: Usando el programa dd (Método válido tambien para CentOS 7/8 y RHEL 7/8)

1. Descargamos la imagen .iso del ambiente del examen desde [aquí](https://static.redhat.com/downloads/training-certification/rhrexboot.iso)  
2. Identificamos nuestra unidad USB mediante el comando **lsblk**:  

```bash
$ lsblk
NAME            MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda               8:0    0 931.5G  0 disk 
├─sda1            8:1    0    16M  0 part 
└─sda2            8:2    0 931.5G  0 part 
sdb               8:16   0 447.1G  0 disk 
├─sdb1            8:17   0   200M  0 part /boot/efi
├─sdb2            8:18   0     1G  0 part /boot
└─sdb3            8:19   0   446G  0 part 
  ├─pc-root 253:0    0    81G  0 lvm  /
  ├─pc-swap 253:1    0     2G  0 lvm  [SWAP]
  └─pc-home 253:2    0   363G  0 lvm  /home
sdc               8:32   1    15G  0 disk 
├─sdc1            8:33   1     8M  0 part 
└─sdc2            8:34   1    15G  0 part /run/media/usuario/New Volume
nvme0n1         259:0    0   477G  0 disk 
├─nvme0n1p1     259:1    0   549M  0 part 
├─nvme0n1p2     259:2    0 475.9G  0 part 
└─nvme0n1p3     259:3    0   519M  0 part
```

Aquí por ejemplo mi unidad USB sería la **sdc** (puede variar dependiendo de cuantos dispositivos de almacenamiento tengas instalados en tu PC)  

3. Como **sudo** ejecutamos el comando **dd** donde "if=/path" es la ruta completa donde se encuentra la imagen .iso y "of=/dev/sdc" es la unidad USB destino.  

```bash
$ sudo dd if=/home/usuario/Descargas/rhrexboot.iso of=/dev/sdc bs=512k
2417+1 records in
2417+1 records out
1267597312 bytes (1.3 GB, 1.2 GiB) copied, 0.483596 s, 2.6 GB/s
```

4. Ejecutamos de nuevo el comando **lsblk** para ver las particiones hechas por el comando dd (sdc1, sdc2, sdc3).  

```bash
$ lsblk
NAME            MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda               8:0    0 931.5G  0 disk 
├─sda1            8:1    0    16M  0 part 
└─sda2            8:2    0 931.5G  0 part 
sdb               8:16   0 447.1G  0 disk 
├─sdb1            8:17   0   200M  0 part /boot/efi
├─sdb2            8:18   0     1G  0 part /boot
└─sdb3            8:19   0   446G  0 part 
  ├─pc-root 253:0    0    81G  0 lvm  /
  ├─pc-swap 253:1    0     2G  0 lvm  [SWAP]
  └─pc-home 253:2    0   363G  0 lvm  /home
sdc               8:32   1    15G  0 disk 
├─sdc1            8:33   1   1.2G  0 part 
├─sdc2            8:34   1     8M  0 part 
└─sdc3            8:35   1    17M  0 part 
nvme0n1         259:0    0   477G  0 disk 
├─nvme0n1p1     259:1    0   549M  0 part 
├─nvme0n1p2     259:2    0 475.9G  0 part 
└─nvme0n1p3     259:3    0   519M  0 part 
```

5. Desconectamos la unidad USB y la conectamos de nuevo, si ejecutamos el comando **lsblk** veremos ahora el punto de montaje de la unidad:  

```bash
$ lsblk
NAME          MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda             8:0    0 931.5G  0 disk 
├─sda1          8:1    0    16M  0 part 
└─sda2          8:2    0 931.5G  0 part 
sdb             8:16   0 447.1G  0 disk 
├─sdb1          8:17   0   200M  0 part /boot/efi
├─sdb2          8:18   0     1G  0 part /boot
└─sdb3          8:19   0   446G  0 part 
  ├─pc-root
  │           253:0    0    81G  0 lvm  /
  ├─pc-swap
  │           253:1    0     2G  0 lvm  [SWAP]
  └─pc-home
              253:2    0   363G  0 lvm  /home
sdc             8:32   1    15G  0 disk 
├─sdc1          8:33   1   1.2G  0 part /run/media/usuario/RHRE-20200630
├─sdc2          8:34   1     8M  0 part 
└─sdc3          8:35   1    17M  0 part 
nvme0n1       259:0    0   477G  0 disk 
├─nvme0n1p1   259:1    0   549M  0 part 
├─nvme0n1p2   259:2    0 475.9G  0 part 
└─nvme0n1p3   259:3    0   519M  0 part 
```

6. Ahora iniciamos el disco USB con el ambiente del examen  

## Iniciando el disco USB con el ambiente del examen

***

1. Reinicia tu PC, y ve al Boot Menu, Selecciona el disco de inicio como USB Flash Drive.  
{{< admonition note >}}
Depediendo del computador, la tecla para entrar al boot menu puede ser diferente, las teclas mas comunes son F2, F4, F11, F12, delete, enter, etc.  
{{< /admonition >}}
2. En la pantalla de selección Boot order - puede ser diferente en varios computadores, la típica pantalla puede ser la siguiente:  
![Boot Order](/images/redhat-online-exams/boot-0.png "Pantalla boot order")
3. Esperamos que cargue el examen  
![Boot exam](/images/redhat-online-exams/boot-1.png "Iniciando examen Red Hat")
4. Click OK en la pantalla de bienvenida.  
5. Preparamos el ambiente del examen en la pantalla Settings configurando por ejemplo el mouse, teclado, region/idioma, sonido, etc.  
![Settings](/images/redhat-online-exams/boot-2.png "Configurando opciones")
6. Vamos a la opción Network para asegurarnos que haya tomado los párametros de la red, y que esté conectado a Internet.  
![Network](/images/redhat-online-exams/network.png "Parámetros de Red")
7. Después de verificar que estemos conectados a Internet, cerramos la pantalla Settings para conectarnos a la página Red Hat Individual Exam Scheduler.  
8. Iniciamos sesión con el usuario y contraseña de nuestra cuenta redhat.com. Debe ser la misma con la que se adquirió el examen.  
9.  Una vez iniciado el ambiente del examen, vamos a ejecutar la opción Compatibility Check para verificar que los requerimientos del sistema son correctos.  
![Red Hat Individual Exam Scheduler](/images/redhat-online-exams/boot-3.png "Red Hat Individual Exam Scheduler")
10. En caso de necesitar ayuda, tenemos la opción **Raise a live chat with support.**  
![Chat Support](/images/redhat-online-exams/boot-4.png "Raise a live chat with support")
11. En la sección **Your Exams** se podrán los examanes agendados dentro de las próximas 24 horas. Haz click en **Your Exams** para comenzar con el examen.  

Espero les haya gustado, ¡hasta la próxima!

## Referencias

***

- [Red Hat Launches Remote Certification Exams](https://www.redhat.com/en/about/press-releases/red-hat-launches-remote-certification-exams)  

- [Learn more about Red Hat Training & Certification Ways to Test](https://www.redhat.com/en/services/certification/ways-to-test)  

- [Getting Ready for your Red Hat Remote Exam](https://learn.redhat.com/jfvwy86652/attachments/jfvwy86652/certification_resources/2/3/Getting%20Ready%20for%20your%20Red%20Hat%20Remote%20Exam_4_Aug_20.pdf)  

- [Red Hat Blog: Red Hat certification remote exams now available](https://www.redhat.com/en/blog/red-hat-certification-remote-exams-now-available)  

## Apoya este Proyecto!!!

Si te pareció útil este artículo y el proyecto en general, considera brindarme un café :)

<style>.bmc-button img{height: 34px !important;width: 35px !important;margin-bottom: 1px !important;box-shadow: none !important;border: none !important;vertical-align: middle !important;}.bmc-button{padding: 7px 15px 7px 10px !important;line-height: 35px !important;height:51px !important;text-decoration: none !important;display:inline-flex !important;color:#ffffff !important;background-color:#5F7FFF !important;border-radius: 8px !important;border: 1px solid transparent !important;font-size: 24px !important;letter-spacing: 0.6px !important;box-shadow: 0px 1px 2px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;margin: 0 auto !important;font-family:'Cookie', cursive !important;-webkit-box-sizing: border-box !important;box-sizing: border-box !important;}.bmc-button:hover, .bmc-button:active, .bmc-button:focus {-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;text-decoration: none !important;box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;opacity: 0.85 !important;color:#ffffff !important;}</style><link href="https://fonts.googleapis.com/css?family=Cookie" rel="stylesheet"><a class="bmc-button" target="_blank" href="https://www.buymeacoffee.com/enmanuelmoreira"><img src="https://cdn.buymeacoffee.com/buttons/bmc-new-btn-logo.svg" alt="Buy me a coffee"><span style="margin-left:5px;font-size:24px !important;">Buy me a coffee</span></a>
