---
title: "Como Instalar os Drivers Propietários AMDGPU Radeon no RHEL 8 / Centos 8"
date: 2020-10-01
lastmod: 2020-10-01
author: Enmanuel Moreira
description: "Para ninguém é um segredo o suplício que pasamos os usuários Linux com as placas gráficas da AMD, pelo pouco suporte (interés mais bem) das grandes companhias de produtos gráficos para desenvolver drivers decentes e poder forcener suporte para este sistema operacional. Tive alguns problemas para instalar o meu editor de videos favorito DaVinci Resolve no CentOS que não reconhecia o componente OpenCL, o que me obrigou a investigar o porquê. "
draft: false

resources:
- name: "featured-image"
  src: "featured-image.jpg"

categories: ["Linux"]
tags: ["tutorial"]

lightgallery: true
---

<!--more-->

Olá Pessoal, tudo bem?

Para ninguém é um segredo o suplício que pasamos os usuários Linux com as placas gráficas da AMD, pelo pouco suporte (interés mais bem) das grandes companhias de produtos gráficos para desenvolver drivers decentes e poder forcener suporte para este sistema operacional. Tive alguns problemas para instalar o meu editor de videos favorito DaVinci Resolve no CentOS que não reconhecia o componente OpenCL, o que me obrigou a investigar o porquê.  

O software Radeon da AMD suporta pocas distros, entre as quais estão Ubuntu e RHEL, pelo que no seguinte artigo vou te mostrar como instalar os drivers privativos de AMDGPU no CentOS 8.  

## Placas Gráficas Suportadas

***

Segundo o website da [AMD](https://www.amd.com/en/support/kb/release-notes/rn-amdgpu-unified-linux-20-20), estão suportadas as seguintes placas:  

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

## Instalando Prerrequisitos

***

O primeiro que devemos fazer, é instalar o Repositório EPEL e habilitar o Repositório PowerTools:  

```bash
sudo dnf install epel-release
sudo dnf config-manager --enable PowerTools
```

Agora vamos instalar os pacotes necessários para que se poda compilar o driver:  

```bash
sudo dnf install -y gcc make kernel-headers-`uname -r` kernel-devel-`uname -r` kernel-modules-extra
```

## Baixando o Driver Radeon

***

Baixmaos o driver desde a página:  

<https://www.amd.com/en/support/kb/release-notes/rn-amdgpu-unified-linux-20-20>:  

```bash
wget https://drivers.amd.com/drivers/linux/amdgpu-pro-20.20-1089974-rhel-8.2.tar.xz
```

Vamos desempacotar:  

```bash
tar xfJv https://drivers.amd.com/drivers/linux/amdgpu-pro-20.20-1089974-rhel-8.2.tar.xz
```

E trocamos o nome à pasta a **amdgpu**

```bash
mv amdgpu-pro-20.20-1089974-rhel-8.2 amdgpu
```

## Criando o Repositório Local

***

O instalador vai (óbvio) todos os pacotes necessários para que os drivers funcionem. Vamos criar um repositório local o qual vai conter a pasta onde se encotrar os arquivos rpm.  

Abrimos o seu editor de textos favorito (usarei vim):  

```bash
sudo vim /etc/yum.repos.d/amdgpu.repo
```

E colamos o seguinte conteúdo:  

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

Onde **baseurl=file:///var/local/amdgpu/** vamos troca-lô pela pasta donde se encontra a pasta dos arquivos rpm **amdgpu**.  

## Instalando o Driver Radeon

***

Entramos na pasta **amdgpu**:  

```bash
cd amdgpu
```

Executamos o script de instalação:  

```bash
sudo ./amdgpu-install
```

Vai demorar alguns minutos em instalar e contruir o driver, uma vez instalado, reiniciamos o sistema:  

```bash
sudo systemctl reboot
```

Uma vez o sistema seja reiniciado, vamos istalar os componentes da OpenCL:

- Se tivessemos uma placa gráfica POLARIS ou anterior (a minha é uma RX560X):

```bash
sudo dnf install ibdrm-amdgpu libdrm-amdgpu-common clinfo-amdgpu-pro opencl-amdgpu-pro-comgr amdgpu-pro-core opencl-orca-amdgpu-pro-icd libopencl-amdgpu-pro
```

- Se a nossa gráfica fosse uma VEGA ou superior:  

```bash
sudo dnf install libdrm-amdgpu libdrm-amdgpu-common clinfo-amdgpu-pro opencl-amdgpu-pro-comgr amdgpu-pro-core opencl-amdgpu-pro-icd libopencl-amdgpu-pro
```

Já com esses passos, teremos instalados os drivers.  

Espero que tinham gostado, até a próxima!  

## Referencias

***

- [Radeon™ Software for Linux® installation instructions](https://amdgpu-install.readthedocs.io/en/latest/)
