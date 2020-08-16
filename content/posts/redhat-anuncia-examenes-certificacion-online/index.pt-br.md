---
title: "Red Hat anuncia os testes de Certificação Online"
date: 2020-08-08
lastmod: 2020-08-09
author: Enmanuel Moreira
description: "Boas notícias para quem ainda não puderam fazer os testes de certificação de Red Hat (lembremos que os testes de certificação de Red Hat só se podem fazer em um Centro Autorizado), e além da situação atual com o COVID-19, finalmente Red Hat anunciou a disponibilidade de alguns dos testes para poder ser feitos em casa/escritório."
draft: false

resources:
- name: "featured-image"
  src: "featured-image.png"

categories: ["Certificações"]
tags: ["redhat","certificações","sysadmin","rhcsa","rhce","rhel"]

lightgallery: true
---


<!--more-->

Olá Pessoal, tudo bem?

Boas notícias para quem ainda não puderam fazer os testes de certificação de Red Hat (lembremos que os testes de certificação de Red Hat só se podem fazer em um Centro Autorizado), e além da situação atual com o COVID-19, finalmente Red Hat [anunciou](https://www.redhat.com/en/about/press-releases/red-hat-launches-remote-certification-exams), a disponibilidade de alguns dos testes para poder ser feitos em casa/escritório.  

## Testes Disponíveis  

***

Em 3 de agosto de 2020, estarão disponíveis os seguintes testes para ser feitos on line:

- [Red Hat Certified System Administrator (RHCSA) exam (EX200V8K)](https://www.redhat.com/en/services/training/ex200-red-hat-certified-system-administrator-rhcsa-exam)

- [Red Hat Certified Engineer (RHCE) exam (EX294V8K)](https://www.redhat.com/en/services/training/ex294-red-hat-certified-engineer-rhce-exam-red-hat-enterprise-linux-8)

- [Red Hat Certified Specialist in OpenShift Administration exam (EX280V42K)](https://www.redhat.com/en/services/training/ex280-red-hat-certified-specialist-in-openshift-administration-exam)

- [Red Hat Certified Specialist in OpenShift Application Development exam (EX288V42K)](https://www.redhat.com/en/services/training/ex288-red-hat-certified-specialist-openshift-application-development-exam)

## Requisitos Técnicos

***

Segundo o documento [Getting Ready for your Red Hat Remote Exam](https://learn.redhat.com/jfvwy86652/attachments/jfvwy86652/certification_resources/2/3/Getting%20Ready%20for%20your%20Red%20Hat%20Remote%20Exam_4_Aug_20.pdf), se vão precisar cumplir con as seguintes condições:  

- Um PC com só um monitor ativo, o uma Laptop com processador Intel/AMD. No caso da laptop, se vai permitir ligar um monitor externo, porém, a laptop deverá permanecer fechada ao momento de fazer o teste.

- 4GB de memoria RAM como mínimo.

- Unidade USB de 8GB mínimo de armazenamento.
{{< admonition warning >}}
A Unidade USB precisa ser apagada completamente para criar o ambiente do teste, é recomendado fazer um backup de todos os dados.
{{< /admonition >}}

- Uma Webcam externa com ao menos 1 m. de cabo.

- É permitido um USB Hub para ligar mouse, teclado, som e microfone.

- Mouse e Teclado. Podem ser externos e deven ser de cabo (não são permitidos mouses ou teclados wireless) e no caso se ser usada uma laptop. a mesma deverá permanecer fechada ao momento de fazer o teste.

- Som y microfone. Verificar que o microfone não esteja mute antes de fazer o teste.

- Conexão a Internet com velocidades mínimas de download de 768Kbps e upload de 512Kbps.

- Conexão a rede por cabo. Não se permite conexão pelo Wi-Fi, isto para asegurar a quaalidade da transmissão de dados durante o teste.
  
- Se o teste fosse feito em uma Laptop, a bateria da mesma deve estar carregada o suficiente e estar ligada ao carregador durante o teste.

## Criação do ambiente do teste

***

Para cria o ambiente do teste, precisamos de três coisas:  

- Uma Unidade USB de 8 GB de capacidade mínimo.  
- O software Fedora Image Writer.  
- A imagem .iso que contém o ambiente do teste.  

{{< admonition tip >}}
Precisam-se privilegios de Administrador do sistema para poder executar todos os pasos a seguir. Deves de obter privilegios de Administrador da tua equipa de TI se estas a usar um PC fornecido pela empresa.
{{< /admonition >}}

### Windows 7, 8.1 / 10

1. Descarregamos a imagem .iso do ambiente do teste desde [aquí](https://static.redhat.com/downloads/training-certification/rhrexboot.iso)

2. Conectamos a unidade USB ao computador.

3. Descarregamos e instalamos o software Fedora Media Writer desde [aquí](https://getfedora.org/fmw/FedoraMediaWriter-win32-latest.exe)

![Licença](/images/redhat-online-exams/fedora-media-writer-0.png "Licença do Programa de Instalação")

![Localização](/images/redhat-online-exams/fedora-media-writer-1.png "Localização da Instalação")

![Instalação Completada](/images/redhat-online-exams/fedora-media-writer-2.png "Instalação Completada")

![Final](/images/redhat-online-exams/fedora-media-writer-3.png "Finalizando Instalador")

4. Abrimos o software Fedora Media Writer para escrever a imagen .iso do ambiente do teste:  
a. Selecionamos **Imagem Personalizada**  
![FIW](/images/redhat-online-exams/fedora-media-writer-4.png "Imagem Personalizada")
b. Escolhemos a imagem .iso onde tinhamos descarregado, depois click em Abrir.  
![FIW](/images/redhat-online-exams/fedora-media-writer-5.png "A Escolher Imagem")
c. Se a unidade USB se encontrar conectada ao computador, o Fedora Media Writer vai mostrar a unidade para criar o ambiente do teste.  
{{< admonition warning >}}
O Fedora Media Writes apagará todos os dados da unidade USB para criar o ambiente do teste. Faz um backup de todos os dados antes de continuar.  
{{< /admonition >}}
![FIW](/images/redhat-online-exams/fedora-media-writer-6.png "A Escolher Imagem")  
d. Click em Escrever ao disco, para começar o processo de escritura.  
![FIW](/images/redhat-online-exams/fedora-media-writer-7.png "A Escrever imagem")
![FIW](/images/redhat-online-exams/fedora-media-writer-8.png "Comprobando datos")
e. Finalizamos o Fedora Media Writer  
![FIW](/images/redhat-online-exams/fedora-media-writer-9.png "Final")

5. Ahora iniciamos o disco USB com o ambiente do teste.  

### Fedora 30 - Fedora 32

{{< admonition info >}}
Precisam-se privilegios de root ou sudo para poder executar as operações seguientes.  
{{< /admonition >}}

#### Método 1: Fedora Media Writer

1. Descarregamos a imagem .iso do ambiente do examen desde [aquí](https://static.redhat.com/downloads/training-certification/rhrexboot.iso)
2. Instalamos o Fedora Image Writer desde Software:  
![Software](/images/redhat-online-exams/fedora-0.png "Aplicação Software")
![Software](/images/redhat-online-exams/fedora-1.png "Procuramos o software Fedora Media Writer")
![Instalação](/images/redhat-online-exams/fedora-2.png "Instalamos o Fedora Media Writer")
![Fedora Media Writer](/images/redhat-online-exams/fedora-3.png "Executamos o Fedora Media Writer")
3. Escolhemos a imagem .iso onde tinhamos descarregado, logo click em Abrir e veremos o ecrã Escrever Imagem Personalizada.  
![Imagem](/images/redhat-online-exams/fedora-4.png "Imagem Personalizada")
![Chat Support](/images/redhat-online-exams/fedora-5.png "A Escolher Imagem")
4. Iniciamos a escritura do disco USB, devemos de forcener a nossa senha de root.  
![Chat Support](/images/redhat-online-exams/fedora-6.png "A Escrever ao disco")
5. Uma vez tinha acabado o processo, iniciamos o disco USB com o ambiente do examen.  

#### Método 2: A Usar o programa dd (Método válido tambien para CentOS 7/8 y RHEL 7/8)

1. Descarregamos a imagem .iso do ambiente do examen desde [aquí](https://static.redhat.com/downloads/training-certification/rhrexboot.iso)  
2. Identificamos a nossa unidade USB mediante o comando **lsblk**:  

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

Neste caso, por exemplo, a minha unidade USB seria a **sdc** (pode ser diferente, a depender de quantos dispotivos estiverem instalados no seu PC)  

3. Com privilegios de **sudo** executamos o comando **dd** onde "if=/path" é a rota completa aonde se encontrar a imagem .iso e "of=/dev/sdc" é a rota da unidade USB destino.  

```bash
$ sudo dd if=/home/usuario/Descargas/rhrexboot.iso of=/dev/sdc bs=512k
2417+1 records in
2417+1 records out
1267597312 bytes (1.3 GB, 1.2 GiB) copied, 0.483596 s, 2.6 GB/s
```

4. Executamos de novo o comando **lsblk** para ver as partições feitas pelo comando dd (sdc1, sdc2, sdc3).  

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

5. Desconectamos a unidade USB e a conectamos de novo, se executamos o comando **lsblk** vamos ver agora o ponto de montagem da unidade:  

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

6. Ahora iniciamos o disco USB com o ambiente do examen  

## Iniciando o disco USB com o ambiente do examen

***

1. Reinicia o teu PC, e vai ao Boot Menu, Seleciona o disco de inicio como USB Flash Drive.  
{{< admonition note >}}
Dependendo do teu computador, a tecla para entrar ao boot menu pode ser diferente, as teclas mais comuns são F2, F4, F11, F12, delete, enter, etc.  
{{< /admonition >}}
2. Na ecrã de seleção Boot order - pode ser diferente em vários computadores, o típico ercã pode ser o seguinte:  
![Boot Order](/images/redhat-online-exams/boot-0.png "Ecrã boot order")
3. Esperamos a que carregue o teste.  
![Boot exam](/images/redhat-online-exams/boot-1.png "A Iniciar teste Red Hat")
4. Click OK no ecrã de bem-vinda.  
5. Preparamos o ambiente do teste no ecrã Settings, vamos configurar por exemplo o mouse, teclado, região/lingua, som, etc.  
![Settings](/images/redhat-online-exams/boot-2.png "Configurando opções")
1. Vamos à opção Network para asegurarnos que tinha tomado os parámetros da rede e que o computador esteja conectado à Internet.  
![Network](/images/redhat-online-exams/network.png "Parámetros de Rede")
7. Uma vez que tinhamos verificado a conexão à Internet, fechamos a janela Settings para conectarnos ao website da Red Hat Individual Exam Scheduler.  
8. Fazemos Login com o usuario e senha da nossa conta redhat.com. Deve ser a mesma com a que se comprou o teste.  
9. Uma vez dentro do ambiente do teste, executamos a opção Compatibility Check para verificar que os requisitos do sistema estiverem correctos.  
![Red Hat Individual Exam Scheduler](/images/redhat-online-exams/boot-3.png "Red Hat Individual Exam Scheduler")
10. No caso de precisar ajuda, teremos a opção **Raise a live chat with support.**  
![Chat Support](/images/redhat-online-exams/boot-4.png "Raise a live chat with support")
11. Na secção **Your Exams** podem ser ver os testes agendados dentro das próximas 24 horas. Faz click em **Your Exams** para començar com o teste.  

Espero que tinham gostado deste tutorial, até a próxima!

## Referencias

***

- [Red Hat Launches Remote Certification Exams](https://www.redhat.com/en/about/press-releases/red-hat-launches-remote-certification-exams)  

- [Learn more about Red Hat Training & Certification Ways to Test](https://www.redhat.com/en/services/certification/ways-to-test)  

- [Getting Ready for your Red Hat Remote Exam](https://learn.redhat.com/jfvwy86652/attachments/jfvwy86652/certification_resources/2/3/Getting%20Ready%20for%20your%20Red%20Hat%20Remote%20Exam_4_Aug_20.pdf)  

- [Red Hat Blog: Red Hat certification remote exams now available](https://www.redhat.com/en/blog/red-hat-certification-remote-exams-now-available)  
