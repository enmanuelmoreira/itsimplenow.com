---
title: "Congelamientos aleatórios em Linux com Microprocessadores AMD"
date: 2021-05-11
lastmod: 2021-05-11
author: Enmanuel Moreira
description: "Se tens um microprocessador AMD e usas o Linux, com certeza tinhas experimentado o seguinte: Estás conforvável a trabalhar e da nada o ecrã congela, o teclado não funciona e desligar o sistema com segurança (se tinhas usado as teclas mágicas Alt + Impr pant + REISUB sabes o que estou a falar) Pois bem, é bastante incomodo que tenhas que reiniciar o sistema cada vez que passa isto, além que poderia danar o sistema de arquivos em um desses reinicios obrigados"
draft: false

resources:
- name: "featured-image"
  src: "featured-image.jpg"

categories: ["Linux"]
tags: ["kernel","linux","amd","opensuse"]

lightgallery: true
---

<!--more-->

## O Problema

***

Se tens um microprocessador AMD e usas o Linux, com certeza tinhas experimentado o seguinte: Estás conforvável a trabalhar e da nada o ecrã congela, o teclado não funciona e desligar o sistema com segurança (se tinhas usado as teclas mágicas Alt + Impr pant + REISUB sabes do que estou a falar) Pois bem, é bastante incomodo que tenhas que reiniciar o sistema cada vez que passa isto, além que poderia danar o sistema de arquivos em um desses reinicios "obrigados".  

## C-States o Modos C

***

Os C-State são a maneira na qual se gestiona a energia da CPU. Foram introduzidos pela primera vez com o microprocessador 486DX4, porém, com o passar do tempo vão se introduzir cada vez mais modos que consumem menos energia.  

- Cada estado (C-State) da CPU utiliza uma quantidade diferente de energia.
- Cada estado (C-State) da CPU tem um impacto no rendimento maneira diferente.
- Quando um núcleo da CPU está inativo, passa-se de um estado a outro superior a desligar vários componentes do microprocessador para consumir menos energía.
- Os estados atuáis vão do 0 ao 10. O zero (0) ou modo ativo normal é o que tem um maior consumo de energía já que a CPU está 100% ligada.
- Ao aumentar o número C, o modo de suspensão da CPU é mais profundo, é dizer, desligam-se mais circuitos e sinais e precisa mais tempo para que a CPU voltar ao modo C0 (para que a CPU acordar). Portanto, ao maior número C-State menor vai ser o consumo.  

Ao dia de hoje, tinham sido incluídas mais caracteristicas para que cada CPU consumir menos energía nos modos de baixo consumo. A ideia es baixar a frecuência do relogio da CPU quando se encontrar com poca carga de trabalho, a reduzir a voltagem ou desligar um núcleo determinado. Por outra parte, deve-se ter em consideração que se precisa de mais tempo para que CPU "acorde" e esteja 100% operacional de novo.  

## Lista de C-States

***

| C-State  | Nombre | Utilidad |
|----------|--------|----------|
| C0 | Estado operativo | O microprocessador está a funcionar com todo o potencial dele |
| C1 | Estado operativo | O microprocessador está a funcionar, mas com os relógios internos detidos na espera de instruções. As interfaces e o APIC continuam a funcionar. |
| C1E | Parada melhorada | Detém todos los relógios internos do microprocessador por software. |
| C2 | Permissão de detenção | Detém os relógios internos e externos da CPU pelo hardware. As interfaces e o APIC continuam a funcionar. |
| C2 | Detenção do relógio | Detém os relógios internos e externos da CPU por hardware. |
| C2E | Permissão de detenção extendido | Detém os relógios principais e além de isso, reduz a voltagem da CPU para reduzir o consumo. As Interfaces e o APIC continuam a funcionar. |
| C3 | Suspensão | Detém todos os relógios internos da CPU. |
| C3 | Suspensão profunda | Detém todos os relógios internos e externos da CPU. |
| C3 | AltVID | Detém todos los relógios internos e reduz a voltagem. |
| C4 | Suspensão profunda | Reduz a voltagem da CPU. |
| C4E/C5 | Suspensão profunda melhorada | Reduz ainda mais a voltagem e desliga a caché da memoria. |
| C6 | Desligado profundo | Reduz a voltagem da CPU a zero. |
| C7 | Poupança de energía profunda | A CPU esvazía a caché L3 e curta a energia dela. Se apaga também a energía do agente do sistema. |
| C7s | - | Sub estado no que a caché L3 esvazia-se de uma vez em lugar de maneira gradual. Os dispositivos I/O ficam no modo poupança de energía. |
| C8 | - | A caché L3 esvazía-se e se curta a energía do PLL. |
| C9 | - | A voltagem da entrada VCC reduz-se ao mínimo. |
| C10| - | A CPU se desliga completamente. |

## AMD e a Gestão de Energía no Linux

***

A coisa é que, os microprocessadores AMD (especificamente os Ryzen) não gestionam muito bem a energía no Linux, sendo esse bug bem cononhecido pela equipa de desenvolvimento do Kernel, como podemos ver neste **[post](https://bugzilla.kernel.org/show_bug.cgi?id=206487)**

Para conhecer mais a fondo o problema, podemos verificar no journal se há algum erro de hardware (se tivessemos habilitado o log persistente):  

```bash
journalctl | grep -i "hardware err"
```

## A Solução

***

{{< admonition tip >}}
Todos os comandos a seguir, devemos de executá-los como **root**
{{< /admonition >}}

Modificamos o arquivo que comtém a configuração do grub:  

```bash
vim /etc/default/grub
```

Na linha **GRUB_CMDLINE_LINUX_DEFAULT**, adicionamos ao final (a respeitar as aspas):  

```bash
processor.max_cstate=1
```

Agora regeneramos o grub:  

```bash
grub2-mkconfig -o /boot/grub/grub.cfg
```

Se tivesses UEFI (trocas  opensuse pela distro que estejas a utilizar):  

```bash
grub2-mkconfig -o /boot/EFI/efi/opensuse/grub.cfg
```

Reiniciamos o sistema:  

```bash
reboot
```

Espero que tinham gostado deste tutorial, até a próxima!

## Referências

***

- [Random freezes with AMD Ryzen on Linux 5.0 (Foro ArchLinux)](https://bbs.archlinux.org/viewtopic.php?id=245608)
- [Random "Freezing" with AMD Ryzen CPUs](https://gist.github.com/diracs-delta/876d74d030f80dc899fc58a244b72df0)
- [What are CPU "C-states" and how to disable them if needed?](https://gist.github.com/wmealing/2dd2b543c4d3cff6cab7)
- [¿Sabes qué son y cómo funcionan los C State en las CPU de Intel? (hardzone.es)](https://hardzone.es/reportajes/que-es/c-state-procesadores-intel/)
