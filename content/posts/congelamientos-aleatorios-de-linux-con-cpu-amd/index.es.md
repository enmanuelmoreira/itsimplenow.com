---
title: "Congelamientos aleatorios en Linux con Procesadores AMD"
date: 2021-05-11
lastmod: 2021-05-11
author: Enmanuel Moreira
description: "Si tienes un procesador AMD y usas Linux, seguramente te ha pasado lo siguiente: Estás comodamente trabajando y de la nada tu pantalla se congela, el teclado no funciona ni siquiera para apagar el sistema de manera segura (los que han usado las teclas mágicas Alt + Impr pant + REISUB saben a que me refiero) Pues bien, como podrás haber experimentado, es bastante molesto que tengas que reiniciar el sistema de tanto en tanto, además de lo que supondría dañar el sistema de archivos en uno de esos reinicios forzados"
draft: false

resources:
- name: "featured-image"
  src: "featured-image.jpg"

categories: ["Linux"]
tags: ["kernel","linux","amd","opensuse"]

lightgallery: true
---

<!--more-->

## El Problema

***

Si tienes un procesador AMD y usas Linux, seguramente te ha pasado lo siguiente: Estás comodamente trabajando y de la nada tu pantalla se congela, el teclado no funciona ni siquiera para apagar el sistema de manera segura (los que han usado las teclas mágicas Alt + Impr pant + REISUB saben a que me refiero) Pues bien, como podrás haber experimentado, es bastante molesto que tengas que reiniciar el sistema de tanto en tanto, además de lo que supondría dañar el sistema de archivos en uno de esos reinicios "forzados"  

## C-States o Modos C

***

Los C-State es la manera en que se gestiona la energía de la CPU. Se introdujeron por primera vez con el procesador 486DX4, pero según va pasando el tiempo cada vez se introducen nuevos modos que consumen menos energía.  

- Cada estado (C-State) de la CPU utiliza una cantidad diferente de energía.
- Cada estado (C-State) de la CPU impacta en el rendimiento de manera diferente.
- Cuando un núcleo de la CPU está inactivo, se pasa de un estado a otro superior apagando varios componentes del procesador para ahorrar energía.
- Los estados actuales van del 0 al 10. El 0 o modo activo normal es el que tiene un mayor consumo de energía ya que la CPU está 100% encendida.
- Al aumentar el número C, el modo de suspensión de la CPU es más profundo, es decir, se apagan más circuitos y señales y se requiere más tiempo para que la CPU vuelva al modo C0, es decir, para que se despierte. Por tanto, a mayor número C-State menor será el consumo.  

Al día de hoy, han sido incorporadas más caracteristicas para que cada CPU ahorre energía en modos de bajo consumo. La idea es bajar la frecuencia del reloj de la CPU cuando se encuentre con poca carga de trabajo, reduciendo el voltaje o apagar un núcleo determinado. Por otra parte, se debe tener en cuenta que se requiere de más tiempo para que la CPU "despierte" y esté 100% operacional de nuevo.  

## Lista de C-States

***

| C-State  | Nombre | Utilidad |
|----------|--------|----------|
| C0 | Estado operativo | El procesador está funcionando con todo su potencial |
| C1 | Estado operativo | El procesador está funcionando, pero con los relojes internos detenidos en espera de instrucciones. Las interfaces y APIC siguen funcionando. |
| C1E | Parada mejorada | Detiene todos los relojes internos del procesador por software. |
| C2 | Permiso de detención | Detiene los relojes internos y externos de la CPU por hardware. Las interfaces y APIC siguen funcionando. |
| C2 | Detención del reloj | Detiene los relojes internos y externos de la CPU por hardware. |
| C2E |Permiso de detención extendido | Detiene los relojes principales y además reduce el voltaje de la CPU para reducir el consumo. Interfaces y APIC siguen funcionando. |
| C3 | Suspensión | Detiene todos los relojes internos de la CPU. |
| C3 | Suspensión profunda | Detiene todos los relojes internos y externos de la CPU. |
| C3 | AltVID | Detiene todos los relojes internos y reduce el voltaje. |
| C4 | Suspensión profunda | Reduce el voltaje de la CPU. |
| C4E/C5 | Suspensión profunda mejorada | Reduce todavía más el voltaje y apaga la caché de la memoria. |
| C6 | Apagado profundo | Reduce el voltaje de la CPU a cero. |
| C7 | Ahorro de energía profundo | La CPU vacía la caché L3 y corta su energía. Se elimina también la energía del agente de sistema. |
| C7s | - | Sub estado en el que la caché L3 se vacía de golpe en lugar de escalonadamente. Se ponen en ahorro de energía los dispositivos I/O. |
| C8 | - | La caché L3 se vacía y se corta la energía del PLL. |
| C9 | - | El voltaje de entrada VCC se reduce al mínimo. |
| C10| - | La CPU se apaga completamente. |

## AMD y la Gestión de Energía en Linux

***

El meollo del asunto es que los procesadores AMD (especificamente Ryzen) no manejan del todo bien la gestión de energía en Linux, y este bug es bien conocido por el equipo de desarrollo del Kernel según podemos ver en este **[hilo](https://bugzilla.kernel.org/show_bug.cgi?id=206487)**

Para conocer más a fondo el problema, podemos verificar en el journal si hay algún error de hardware (si tuvieramos habilitado el log persistente):  

```bash
journalctl | grep -i "hardware err"
```

## La Solución

***

{{< admonition tip >}}
Todos los comandos a continuación, debemos ejecutarlos como usuario **root**
{{< /admonition >}}

Modificamos el archivo que contiene la configuración del grub:  

```bash
vim /etc/default/grub
```

En la línea **GRUB_CMDLINE_LINUX_DEFAULT**, agregamos al final (respetando las comillas):  

```bash
processor.max_cstate=1
```

Ahora regeneramos el grub:  

```bash
grub2-mkconfig -o /boot/grub/grub.cfg
```

Si tienes UEFI (cambias opensuse por la distro de Linux que estés utilizando):  

```bash
grub2-mkconfig -o /boot/EFI/efi/opensuse/grub.cfg
```

Reiniciamos el sistema:  

```bash
reboot
```

Espero les haya gustado este tutorial, ¡hasta la próxima!

## Referencias

***

- [Random freezes with AMD Ryzen on Linux 5.0 (Foro ArchLinux)](https://bbs.archlinux.org/viewtopic.php?id=245608)
- [Random "Freezing" with AMD Ryzen CPUs](https://gist.github.com/diracs-delta/876d74d030f80dc899fc58a244b72df0)
- [What are CPU "C-states" and how to disable them if needed?](https://gist.github.com/wmealing/2dd2b543c4d3cff6cab7)
- [¿Sabes qué son y cómo funcionan los C State en las CPU de Intel? (hardzone.es)](https://hardzone.es/reportajes/que-es/c-state-procesadores-intel/)

## Apoya este Proyecto!!!

Si te pareció útil este artículo y el proyecto en general, considera brindarme un café :)

<style>.bmc-button img{height: 34px !important;width: 35px !important;margin-bottom: 1px !important;box-shadow: none !important;border: none !important;vertical-align: middle !important;}.bmc-button{padding: 7px 15px 7px 10px !important;line-height: 35px !important;height:51px !important;text-decoration: none !important;display:inline-flex !important;color:#ffffff !important;background-color:#5F7FFF !important;border-radius: 8px !important;border: 1px solid transparent !important;font-size: 24px !important;letter-spacing: 0.6px !important;box-shadow: 0px 1px 2px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;margin: 0 auto !important;font-family:'Cookie', cursive !important;-webkit-box-sizing: border-box !important;box-sizing: border-box !important;}.bmc-button:hover, .bmc-button:active, .bmc-button:focus {-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;text-decoration: none !important;box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;opacity: 0.85 !important;color:#ffffff !important;}</style><link href="https://fonts.googleapis.com/css?family=Cookie" rel="stylesheet"><a class="bmc-button" target="_blank" href="https://www.buymeacoffee.com/enmanuelmoreira"><img src="https://cdn.buymeacoffee.com/buttons/bmc-new-btn-logo.svg" alt="Buy me a coffee"><span style="margin-left:5px;font-size:24px !important;">Buy me a coffee</span></a>
