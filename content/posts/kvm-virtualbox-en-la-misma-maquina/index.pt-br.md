---
title: "KVM e VirtualBox na mesma Máquina"
date: 2020-09-06
lastmod: 2020-09-06
author: Enmanuel Moreira
description: "KVM e VirtualBox supostamente não podem convivir na mesma máquina, é dizer, o VirtualBox não poderia ser executado enquanto os módulos da KVM estejam carregados na memoria. KVM pode funcionar sem problema nenhum ainda o VirtualBox estivesse instalado na mesma máquina.  "
draft: false

resources:
- name: "featured-image"
  src: "featured-image.png"

categories: ["Linux","Virtualização"]
tags: ["linux","virtualização","virtualbox","kvm"]

lightgallery: true
---

<!--more-->

Olá Pessoal, tudo bem?

KVM e VirtualBox supostamente não podem convivir na mesma máquina, é dizer, o VirtualBox não poderia ser executado enquanto os módulos da KVM estejam carregados na memoria. KVM pode funcionar sem problema nenhum ainda o VirtualBox estivesse instalado na mesma máquina.  

Com este panorama, vou te explicar como usar o KVM e o VirtualBox na mesma máquina sem desinstalar nenhum dos dois, ao vivo, sem reiniciar.  

## Sintoma

Ao executar o VirtualBox, poderás ver a seguinte messagem:  

![Mensagem Erro VirtualBox](/images/kvm-virtualbox/virtualbox-error.png "O VirtualBox ão pode iniciar uma una máquina Virtual")

A Messagem diz: "O VirtualBox não pode habilitar a extensão AMD-V. Por favor desabilite a extensão kernel de KVM, recompile o seu kernel e faça um reinício" Vamos calmarnos. Esta messagem é um pouco confusa, não precisas recompilar coisa nenhuma. Vai-nos tomar um par de minutos e não vai ser destrutivo.  

## O Problema

O VirtualBox e KVM não podem funcionar ao mesmo tempo. São como um casal que vivem na mesma casa mas não se podem nem ver. Isto quer dizer que teriamos que desabilitar KVM para que o VirtualBox funcionasse. A questão é: ¿Cómo fazemos isto? A responta é bem simples: ativando y desativando os módulos do kernel de KVM.  

O Linux suporta habilitar y desabilitar módulos do kernel da memória ao vivo, tivendo necesidade nenhuma de reinciar o sistema. Para alcanzar o nosso objetivo, vamos usar os comandos **insmod** y **rmmod**  

## Desabilitando Módulos

Primeiro, vamos ver o kernel. Executamos o comando **lsmod** a procurar quais são os módulos que estão carregados na memória e vamos fazer un filtro por expressões regulares (vbox e kvm):  

```bash
sudo lsmod | grep vbox
```

```bash
vboxnetflt             28672  0
vboxdrv               512000  2 vboxnetadp,vboxnetflt
```

```bash
sudo lsmod | grep kvm
```

```bash
kvm_amd               114688  0
kvm                   823296  1 kvm_amd
```

Como podemos observar, o VirtualBox utiliza os drivers vboxdrv e vboxnetflt, entretanto o KVM utiliza os drivers kvm e kvm_amd (eu tengo uma máquina com processador AMD, se tivesses um processador Intel sería o driver kvm_intel) O seguinte que vamos fazer é desabilitar o módulo do kernal de KVM:  

```bash
sudo rmmod kvm
sudo rmmod kvm_amd
```

Iniciamos qualquier máquina virtual e verificamos se podemos executa-lâ.  

![VirtualBox já funcionando](/images/kvm-virtualbox/virtualbox-funcionando.png "VirtualBox já funcionando")

## Habilitando Módulos

Assim como desabilitamos os módulos, podemos habilita-lôs de novo. Vamos precisar localizar os módulos no disco e carrega-lôs. Podes usar o comando **modprobe**, o qual pode ser muito útil se o a ser carregado tivesse dependências.  

Vamos buscar o módulo com o comando **find**, filtrando a busca apenas para os arquivoscom extensão kvm.ko e kvm-amd.ko do nosso kernel atúal [**$(uname -r)**] e os ordenamos com o comando **sort**:  

```bash
find /lib/modules/$(uname -r) -iname '*kvm*.ko*' | sort
```

```bash
/lib/modules/5.8.13-300.fc33.x86_64/kernel/arch/x86/kvm/kvm-amd.ko.xz
/lib/modules/5.8.13-300.fc33.x86_64/kernel/arch/x86/kvm/kvm-intel.ko.xz
/lib/modules/5.8.13-300.fc33.x86_64/kernel/arch/x86/kvm/kvm.ko.xz
/lib/modules/5.8.13-300.fc33.x86_64/kernel/drivers/gpu/drm/i915/gvt/kvmgt.ko.xz
/lib/modules/5.8.13-300.fc33.x86_64/kernel/drivers/ptp/ptp_kvm.ko.xz
```

Se observarmos os três primeiros resultados, vemos o módulo kvm-amd, kvm-intel e kvm, vamos carregar kvm e vamos escolher dependendo do nosso processador:  

```bash
sudo insmod /lib/modules/$(uname -r)/kernel/arch/x86/kvm/kvm.ko.xz
```

No AMD:  

```bash
sudo insmod /lib/modules/$(uname -r)/kernel/arch/x86/kvm/kvm-amd.ko.xz
```

No Intel:

```bash
sudo insmod /lib/modules/$(uname -r)/kernel/arch/x86/kvm/kvm-intel.ko.xz
```

## Tentando Faze-lô mais Simples: Scripts

Vamos automatizar de uma vez. Precisaremos dois scripts: O primeiro vai desabilitar os módulos de KVM e outro vai habilita-lôs. Se quiseremos ser mais estritos, pararemos o serviço VirtualBox quando estivessemos a usar (na realidad, eu não veio necessário) e inicia-lô depois de terminar alguma máquina virtual de KVM. Verficamos o estatus do serviço:  

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

Então paramos o serviço:  

```bash
sudo systemctl stop vboxdrv
```

### Desabilitando KVM e Habilitando VirtualBox

{{< admonition tip >}}
Se não sabemos onde se encontram os comandos **rmmod** e **insmod**, com o comando which podemos localiza-lôs bem fácil:  

```bash
which rmmod
```

```bash
/usr/sbin/rmmod
```

```bash
which insmod
```

```bash
/usr/sbin/insmod
```

{{< /admonition >}}

Criamos o script para habilitar VirtualBox e desabilitar KVM:  

```bash
vi desabilitar_kvm.sh
```

Cola o seguinte conteúdo (troca por kvm_intel no caso que tivesses um processador Intel), guardamos e saímos do editor:  

```bash
#!/bin/bash
/usr/sbin/rmmod kvm_amd
/usr/sbin/rmmod kvm
systemctl start vboxdrv
```

### Habilitando KVM e Desabilitando VirtualBox

Fazemos um script para habilitar KVM e desabilitar VirtualBox:  

```bash
vi habilitar.sh
```

Colamos o seguinte conteúdo, guardamos e saímos do editor:  

```bash
#!/bin/bash
systemctl stop vboxdrv
/usr/sbin/insmod /lib/modules/$(uname -r)/kernel/arch/x86/kvm/kvm.ko.xz
/usr/sbin/insmod /lib/modules/$(uname -r)/kernel/arch/x86/kvm/kvm-amd.ko.xz
```

Vamos habilitar privilégios de execução:  

```bash
chmod +x desablitar_kvm.sh habilitar_kvm.sh
```

E quando o executemos, tem que ser com privilégios sudo:  

```bash
sudo ./desabilitar_kvm.sh
```

```bash
sudo ./habilitar_kvm.sh
```

Isto é tudo por hoje, espero que tinham gostado, até a próxima!
