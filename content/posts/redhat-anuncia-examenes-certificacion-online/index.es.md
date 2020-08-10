---
title: "Red Hat anuncia sus examenes de Certificación Online"
date: 2020-08-10
lastmod: 2020-08-10
author: Enmanuel Moreira
description: "Hay buenas noticias para todos aquellos que quedaron sin poder presentar los examenes de Red Hat (recordemos que los examenes de certificación de Red Hat solo se pueden hacer en un Centro Autorizado), y viendo la actual situación mundial con el COVID-19, finalmente Red Hat anunció la disponibilidad de algunos sus examenes para ser presentados en casa/oficina."
draft: true

resources:
- name: "featured-image"
  src: "featured-image.png"

categories: ["Certificaciones"]
tags: ["redhat","certificaciones","sysadmin","rhcsa","rhce","rhel"]

lightgallery: true
---

<!--more-->

Hola chiquillos!

Hay buenas noticias para todos aquellos que quedaron sin poder presentar los examenes de Red Hat (recordemos que los examenes de certificación de Red Hat solo se pueden hacer en un Centro Autorizado), y viendo la actual situación mundial con el COVID-19, finalmente Red Hat [anunció](https://www.redhat.com/en/about/press-releases/red-hat-launches-remote-certification-exams), la disponibilidad de algunos sus examenes para ser presentados en casa/oficina.

***

## Exámenes Disponibles  

***

A partir del 3 de agosto de 2020, estarán disponibles los siguientes examenes para realizarlos en linea:

- [Red Hat Certified System Administrator (RHCSA) exam (EX200V8K)](https://www.redhat.com/en/services/training/ex200-red-hat-certified-system-administrator-rhcsa-exam)

- [Red Hat Certified Engineer (RHCE) exam (EX294V8K)](https://www.redhat.com/en/services/training/ex294-red-hat-certified-engineer-rhce-exam-red-hat-enterprise-linux-8)

- [Red Hat Certified Specialist in OpenShift Administration exam (EX280V42K)](https://www.redhat.com/en/services/training/ex280-red-hat-certified-specialist-in-openshift-administration-exam)

- [Red Hat Certified Specialist in OpenShift Application Development exam (EX288V42K)](https://www.redhat.com/en/services/training/ex288-red-hat-certified-specialist-openshift-application-development-exam)

***

## Requerimientos Técnicos

***

Según el documento [Getting Ready for your Red Hat Remote Exam](https://learn.redhat.com/jfvwy86652/attachments/jfvwy86652/certification_resources/2/3/Getting%20Ready%20for%20your%20Red%20Hat%20Remote%20Exam_4_Aug_20.pdf), se necesitarán cumplir con las siguientes condiciones:  

- Una PC con un solo monitor activo, o una Laptop con procesador Intel/AMD. En el caso de la Laptop. Se permitirá conectar un monitor externo, siempre y cuando la pantalla de la laptop se cierre al momento de hacer el examen.

- 4GB de RAM como mínimo.

- Unidad USB de 8GB mínimo de almacenamiento.
{{< admonition >}}
La Unidad de USB necesita ser borrada completamente para crear el ambiente del examen, por lo que se recomienda hacer un respaldo de los datos contenidos en el.
{{< /admonition >}}

- Una Webcam externa que tenga al menos 1 mtr de cable.

- Se permiten un USB Hub para conectar mouse, teclado, sonido y micrófono.

- Mouse y Teclado. Pueden ser externos siempre y cuando sen cableados (no se permiten mouse o teclados inalámbricos) y si se usa una laptop, esta debe estar cerrada al momento de realizar el examen.

- Sonido y micrófono. Se debe verificar que el micrófono no esté en mute al momento de realizar el examen.

- Conexión a Internet con velocidades mínimas de descarga de 768Kbps y subida de 512Kbps.

- Conexión a la Red cableado. No se permite conexión por Wi-Fi para asegurar la calidad de la transmisión durante el examen.
  
- Si se realiza el examen en una Laptop, esta debe tener suficiente carga en la bateria y estar conectada al cargador al momento de realizar el examen.

***

## Configuración del ambiente del examen

Para configurar el ambiente del examen, necesitamos tres cosas: una Unidad USB de 8 GB de capacidad mínimo, el programa Fedora Image Writer y la imagen .iso que contiene el ambiente de la prueba.

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
b. Escogemos la imagen .iso donde la hatamos descargado luego damos click a Abrir y nos llevará a la pantalla de Escribir Imagen Personalizada.  
![FIW](/images/redhat-online-exams/fedora-media-writer-5.png "Seleccionando Imagen")
c. 

## Iniciando el disco USB con el ambiente del examen



## Referencias

- [Red Hat Launches Remote Certification Exams](https://www.redhat.com/en/about/press-releases/red-hat-launches-remote-certification-exams)

- [Learn more about Red Hat Training & Certification Ways to Test](https://www.redhat.com/en/services/certification/ways-to-test)

- [Getting Ready for your Red Hat Remote Exam](https://learn.redhat.com/jfvwy86652/attachments/jfvwy86652/certification_resources/2/3/Getting%20Ready%20for%20your%20Red%20Hat%20Remote%20Exam_4_Aug_20.pdf)

- [Red Hat Blog: Red Hat certification remote exams now available](https://www.redhat.com/en/blog/red-hat-certification-remote-exams-now-available)

- [Getting Ready for your Red Hat Remote Exam](https://learn.redhat.com/t5/Certification-Resources/Getting-Ready-for-your-Red-Hat-Remote-Exam/ba-p/12690)

Espero les haya gustado, ¡hasta la próxima!
