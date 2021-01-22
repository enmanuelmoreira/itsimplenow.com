---
title: "Como Instalar Foreman no CentOS 8 / RHEL 8"
date: 2021-01-07
lastmod: 2021-01-07
author: Enmanuel Moreira
description: ""
draft: true

resources:
- name: "featured-image"
  src: "featured-image.png"

categories: ["Linux","Automação","Tutorial"]
tags: ["redhat","centos","automação","infraestrutura"]
lightgallery: true
---

<!--more-->

## Introdução

***

O [Foreman](https://www.theforeman.org/) é uma ferramenta de código aberto que vai-nos permitir o fornecimento automâtico e configuração de sistemas Linux. Pode ser integrado com diversas ferramentas de automação como o Ansible, Chef, Salt e Puppet, que estão diponíveis em forma de  como plugins e gestionar o ciclo de vida de uma infraestrutura. Neste post vou explicar como instalar o Foreman em um sistema CentOS 8 / RHEL 8.  

## Arquitetura do Foreman

***

Um servodor configurado com o Foreman sempre vai conter uma instância central a qual é responsável de forneceer uma interface web, configurações dos nodos, arquivos de configuração iniciais dos hosts, etc. Porém, se o servidor instalado suporta instalações desacompanhadas então as outras operações precisam ser feitas para automatizar o processo por completo. O Smart Proxy gestiona os serviços remotos e é geralmente instalado com todos os componentes do Foreman para gestionar TFTP, DHCP, DNS, Puppet, Puppet CA, Ansible, Salt, e Chef.

## Smart-Proxy

***

O Smart-Proxy está localizado dentro ou perto de uma máquina que realiza uma função e ajuda ao Foreman a orquestar o proceso de configurar um host novo. Colocar o proxy dentro ou perto do serviço atúal ajuda também a reduizr a latência em uma infraestrutura distribuída de grande tamanho.  

![Arquitetura do Foreman](/images/foreman-installation/foreman_architecture.png "Arquitetura do Foreman")

## Características do Foreman

***

As principais características do Foreman, sáo as seguintes:  

- Descubre, aprovisiona e atualiza uma infraestrutura física completa.  
- Cria e gestiona instâncias em um ambiente de virtualização quanto em uma nuvém privada como pública.  
- Instala sistemas operativos através de, meios locais ou desde templates ou imagens.  
- Controla e fornece relat+orios desde o software de configuração.  
- Agrupa e gestiona os hosts em série, independientemente da localização.  
- Pode-se ver o histórico de mudanças para efeitos de auditoria e troubleshooting.  
- Interfaz gráfica de usuario, JSON REST API e CLI para Linux.  
- Podem-se extender as funcionalidades se for necessário, através de uma robusta arquitectura de plugins.  

Após instalar o Foreman de instalar Foreman, a principal tarefa a executar é a de instalar os plugins que vão formar parte da infraestrutura. Se for usado RHEV como Infraestrutura de virtualização, é necessário especificar os plugins que serão habilitados para aprovisionar e apagar recursos. O mesmo modelo deve ser aplicado para todos o demais softwares de gestão de Infraestrutura como KVM, Xen, CloudStack, OpenStack e outros.  

## Instalando o Foreman

***

### Configurando o nome do host

É necessário configurar um nome de host para o servidor onde vai ser instalado. Vou utilizar o domínio itsimplenow.com, pelo que deves mudar-lô de acordo a tua Infraestrutura atúal.  

```bash
sudo hostnamectl set-hostname foreman.itsimplenow.com
```

Com o seu editor de textos de confianza, vamos modificar o arquivo /etc/hosts para colocar o endereço IP e o nome FQDN do servidor:  

```bash
192.168.0.10 foreman.itsimplenow.com foreman
```

Vamos comprovar se está a tomar o endereço IP e não deja a de localhost (127.0.0.1):  

```bash
ping -c 2 $(hostname -f)
```

```bash
PING foreman.itsimplenow.com (192.168.0.10) 56(84) bytes of data.
64 bytes from foreman.itsimplenow.com (192.168.0.10): icmp_seq=1 ttl=64 time=0.052 ms
64 bytes from foreman.itsimplenow.com (192.168.0.10): icmp_seq=2 ttl=64 time=0.035 ms

--- foreman.itsimplenow.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1023ms
rtt min/avg/max/mdev = 0.035/0.043/0.052/0.010 ms
```

Se o ping da como resultado 127.0.0.1, modifica ou apaga a entrada no arquivo /etc/hosts no casso de estar presente.  

O nome nombre de host configurado pode ser confimado com o comando:  

```bash
host foreman.itsimplenow.com
```

### Instalar e Configurar NTP para Sincronizar a Data e a Hora

***

O Chronyd é a alternativa de sincronização de tempo para sistemas CentOS / RHEL, vamos instalar-la com o seguiente comando:  

```bash
sudo dnf install chrony -y
```

Uma vez feita a instalação, vamos configurar a nossa zona horária:  

```bash
sudo timedatectl set-timezone America/Santiago
```

Iniciamos chronyd e habilitamos para que seja executada ao iniciar o sistema:  

```bash
sudo systemctl enable --now chronyd
```

Sincronizamos a data e a hora:  

```bash
sudo chronyc sources
```

```bash
210 Number of sources = 5
MS Name/IP address         Stratum Poll Reach LastRx Last sample
===============================================================================
#* PHC0                          0   3   377     8  -1479ns[  +11us] +/- 1358ns
^- 13.86.101.172                 3   6   377    77  +2545us[+2410us] +/-   41ms
^- 44.190.6.254                  2   6   377    13   -678us[ -679us] +/-   57ms
^- ns2.nuso.cloud                2   6   377    11  -1875us[-1870us] +/-   46ms
^- linode.ibendit.com            3   6   367    13  +2223us[+2222us] +/-   93ms
```

E agora vamos comprovar a configuração:  

```bash
timedatectl
```

```bash
               Local time: Tue 2020-09-15 16:50:41 -03
           Universal time: Tue 2020-09-15 19:50:41 UTC
                 RTC time: Tue 2020-09-15 19:50:41
                Time zone: America/Santiago (-03, -0300)
System clock synchronized: yes
              NTP service: n/a
          RTC in local TZ: no
```

### Criando os repositórios do Foreman e Puppet

***

#### Puppet

```bash
sudo dnf -y install https://yum.puppet.com/puppet6-release-el-8.noarch.rpm
```

#### Foreman

```bash
sudo dnf -y install https://yum.theforeman.org/releases/2.1/el8/x86_64/foreman-release.rpm
```

### Instalando o Foreman

***

Uma vez que ambos repositórios sejan instalados, vamos instalar o executable de instalação do Foreman:  

```bash
sudo dnf -y install foreman-installer
```

Quando a instalação for feita, vamos configurar os componentes do Foreman. O instalador são do Puppet que instala e configura tudo por nós:  

- Servidor Apache HTTP com SSL (usando um certificado Puppet auto-assinado)
- Execução do Foreman com mod_passenger.
- Configura o Smart Proxy para Puppet, TFTP e SSL.
- Execução do Puppet master com mod_passenger
- Configuração do agente Puppet.
- Servidor TFTP (xinetd em sistemas Red Hat)

O instalador do Foreman executa-se em um modo não interativo por padrão, mas a configuração pode ser personalizada fornecendo agumas das seguintes opções:  

```bash
foreman-installer --help
```

Para que o instalador se execute em modo interativo:  

```bash
sudo foreman-installer -i
```

No caso de instalar-lô com o modo não interativo:  

```bash
sudo foreman-installer
```

```bash
Preparing installation Done
Executing: foreman-rake upgrade:run
foreman-rake upgrade:run finished successfully!
  Success!
  * Foreman is running at https://foreman.itsimplenow.com
      Initial credentials are admin / 7o6gqYujGYZsofj4
  * Foreman Proxy is running at https://foreman.itsimplenow.com:8443
  The full log is at /var/log/foreman-installer/foreman.log
```

Vamos ver a credencial de acesso admin (Initial credentials are admin / 7o6gqYujGYZsofj4) e a url (<https://foreman.itsimplenow.com>)

### Configurando o Firewall

***

Vamos abrir as portas requeridas:  

```bash
sudo firewall-cmd --add-port={22,53,80,443,3000,8140,3306,5432,8443,5910-5930}/tcp --permanent
```

```bash
sudo firewall-cmd --add-port=67-69/udp --permanent
```

Vamos recargar o firewall:  

```bash
sudo firewall-cmd --reload
```

E pronto, já teremos instalado.  

![Foreman Login](/images/foreman-installation/foreman_login.png "Inicio de Sessão")

![Foreman Dashboard](/images/foreman-installation/foreman_dashboard.png "Dashboard")

Espero que tinham gostado deste tutorial, até a próxima!
