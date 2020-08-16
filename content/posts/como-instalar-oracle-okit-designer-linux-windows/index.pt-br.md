---
title: "Como Instalar Okit Designer Tookit no Linux e Windows para desenhar diagramas de Oracle Cloud Infrastructure"
date: 2020-08-07
lastmod: 2020-08-08
author: Enmanuel Moreira
description: "Nesta guia vou explicar como instalar a ferramenta Okit Designer Tookit para desenhar os nossos próprios diagramas de Oracle Cloud Infrastructure."
draft: false

resources:
- name: "featured-image"
  src: "featured-image.png"

categories: ["Linux"]
tags: ["oracle","oci","containers","cloud","redhat","linux","centos","tutoriais","windows","oci-cli","oci-designer"]

lightgallery: true
---

<!--more-->

Olá Pessoal, tudo bem?

Nesta guia vou explicar como instalar a ferramenta Okit Designer Tookit para desenhar os nossos próprios diagramas de Oracle Cloud Infrastructure.

## Okit Designer

***

[OCI designer toolKIT (OKIT)](https://github.com/oracle/oci-designer-toolkit) é um conjunto de ferramentas que permitem desenhar, deployar e visualizar ambientes de OCI através de uma interfaz web, o que permite aos arquitetos e desenhadores cloud contruir uma representação visual da sua infraestrutura e logo poder ser exportada nos seguintes formatos:  

- svg
- png
- jpeg  

Uma vez que se tinha completado o desenho, este pode ser exportado às seguintes ferramentas:  

- Ansible
- Terraform
- OCI Resource Manager  

Isto acelera o deploy de uma infraestrutura completa em OCI.  

Há duas maneiras de instalar Okit Designer: uma com o Vagrant e outra com o Docker. Nesta ocasião o que vamos fazer é instala-lô pelo Docker e Docker Compose para que esteja isolado do nosso sistema.  

## Instalar Requisitos

***

### Fedora 32

***

Antes de instalar o Docker, vamos atualizar o nosso sistema:  

```bash
$ sudo dnf update -y
```

No caso que se tinha atualizado a um kernel novo, devemos reiniciar o sistema:  

```bash
$ sudo reboot
```

Logo, Instalamos o pacote:  

```bash
$ sudo dnf -y install dnf-plugins-core
```

#### Instalar o Git

***

```bash
$ sudo dnf -y install git
```

#### Instalar o Docker

***

Adicionamos o repositório do Docker-CE:  

```bash
$ sudo tee /etc/yum.repos.d/docker-ce.repo<<EOF
[docker-ce-stable]
name=Docker CE Stable - \$basearch
baseurl=https://download.docker.com/linux/fedora/31/\$basearch/stable
enabled=1
gpgcheck=1
gpgkey=https://download.docker.com/linux/fedora/gpg
EOF
```

Atualizamos os repositórios e instalamos o Docker:  

```bash
$ sudo dnf makecache
$ sudo dnf install docker-ce docker-ce-cli containerd.io -y
```

Habilitamos o serviço e o iniciamos:  

```bash
$ sudo systemctl enable --now docker
```

Comprovamos o status do Dcoker e comprovamos se já está se está a executar:  

```bash
$ sudo systemctl status docker
```

Devia de mostrar a seguinte saída:  

```plaintext
● docker.service - Docker Application Container Engine
     Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
     Active: active (running) since Fri 2020-08-07 08:57:50 -04; 6h ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 1345 (dockerd)
      Tasks: 26
     Memory: 126.6M
     CGroup: /system.slice/docker.service
             └─1345 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Aug 07 08:57:48 localhost.localdomain dockerd[1345]: time="2020-08-07T08:57:48.176376583-04:00" level=warning msg="Your kernel>
Aug 07 08:57:48 localhost.localdomain dockerd[1345]: time="2020-08-07T08:57:48.176383213-04:00" level=warning msg="Your kernel>
Aug 07 08:57:48 localhost.localdomain dockerd[1345]: time="2020-08-07T08:57:48.176388723-04:00" level=warning msg="Your kernel>
Aug 07 08:57:48 localhost.localdomain dockerd[1345]: time="2020-08-07T08:57:48.176551002-04:00" level=info msg="Loading contai>
Aug 07 08:57:48 localhost.localdomain dockerd[1345]: time="2020-08-07T08:57:48.984390440-04:00" level=info msg="Default bridge>
Aug 07 08:57:49 localhost.localdomain dockerd[1345]: time="2020-08-07T08:57:49.213838371-04:00" level=info msg="Loading contai>
Aug 07 08:57:50 localhost.localdomain dockerd[1345]: time="2020-08-07T08:57:50.670726531-04:00" level=info msg="Docker daemon">
Aug 07 08:57:50 localhost.localdomain dockerd[1345]: time="2020-08-07T08:57:50.672983824-04:00" level=info msg="Daemon has com>
Aug 07 08:57:50 localhost.localdomain dockerd[1345]: time="2020-08-07T08:57:50.772160936-04:00" level=info msg="API listen on >
Aug 07 08:57:50 localhost.localdomain systemd[1]: Started Docker Application Container Engine.
```

Adicionamos o usuário atual do sistema ao grupo docker e desta maneira podemos executar os comandos do Docker sem usar sudo:  

```bash
$ sudo usermod -aG docker $(whoami)
$ newgrp docker
```

Comprovamos a versão do Docker que temos instalado:  

```bash
$ docker version

Client: Docker Engine - Community
 Version:           19.03.12
 API version:       1.40
 Go version:        go1.13.10
 Git commit:        48a66213fe
 Built:             Mon Jun 22 15:46:56 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.12
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.13.10
  Git commit:       48a66213fe
  Built:            Mon Jun 22 15:44:53 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.2.13
  GitCommit:        7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```

#### Instalar o Docker Compose

***

A versão atual é a 1.26.2, baixamos el executable do Docker Compose e o salvamos na pasta /usr/local/bin:  

```bash
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

Mudamos os privilegios de executação ao Docker-Compose:  

```bash
$ sudo chmod +x /usr/local/bin/docker-compose
```

Comprovamos se está corretamente instalado:  

```bash
$ docker-compose --version
docker-compose version 1.26.2, build eefe0d31
```

### Microsoft Windows 10 Pro

***

#### Ativando Componentes necesarios  

***

No menu Inicio, pesquisamos a ferramenta "Ativar o Desativar Características de Windows":  

![Inicio](/images/oci-okit-linux-windows/win-0.png "Menú Inicio")

Ativamos os seguintes componentes:  

- Containers
- Hyper-V
- Subsistema de Windows para Linux

![Inicio](/images/oci-okit-linux-windows/win-1.png "Ativar o Desativar Características")

Aguardamos a que se instalem os componentes requeridos:  

![Inicio](/images/oci-okit-linux-windows/win-2.png "A Instalar características")

Reiniciamos o sistema:  

![Inicio](/images/oci-okit-linux-windows/win-3.png "Reiniciar")

Una vez que hayamos reiniciado el sistema, instalaremos WSL v2.

#### Instalar o WSL v2  

***

Baixamos o instalador de WSL v2:  

<https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi>

Abrimos o instalador com privilegios de administrador e seguimos os passos:  

![WSLv2](/images/oci-okit-linux-windows/win-4.png "A Abrir Instalador")

![WSLv2](/images/oci-okit-linux-windows/win-5.png "A Finalizar instalação")

Agora, vamos configurar que WSL v2 seja a versão padrão do nosso sistema. Para isto, devemos abrir unaa terminal de PoweShell como Administrador (eu utilizo a versão 7 que podes baixar [aquí](https://github.com/PowerShell/PowerShell/releases/download/v7.0.3/PowerShell-7.0.3-win-x64.msi)) e colocamos o seguinte comando:

```powershell
wsl --set-default-version 2
```

Já com isto acabamos de configurar no nosso sistema.  

#### Instalar o Git

***

Baixamos o instalador do Git:  

- 64 bits: <https://github.com/git-for-windows/git/releases/download/v2.28.0.windows.1/Git-2.28.0-64-bit.exe>

Abrimos o instalador com privilegios de Administrador e vamos instalar com as opções padrão:

![Git Windows](/images/oci-okit-linux-windows/win-6.png "Licença")

![Git Windows](/images/oci-okit-linux-windows/win-7.png "Pasta Destino, o dexicamos na localização padrão")

![Git Windows](/images/oci-okit-linux-windows/win-8.png "Selecionamos os componentes a instalar, deixamos aqueles que estão padrão")

![Git Windows](/images/oci-okit-linux-windows/win-9.png "Icones do Menú Inicio")

![Git Windows](/images/oci-okit-linux-windows/win-10.png "Dexiamos as opções padrão ou podemos trocar o editor de textos padrão")

![Git Windows](/images/oci-okit-linux-windows/win-11.png "Deixamos a opção recomendada")

![Git Windows](/images/oci-okit-linux-windows/win-12.png "Deixamos o executable de ssh que vem com o instalador, no caso de já ter instalado o putty, escolhemos a segunda opção")

![Git Windows](/images/oci-okit-linux-windows/win-13.png "Deixamos padrão a OpenSSL para emitir certificados X509")

![Git Windows](/images/oci-okit-linux-windows/win-14.png "Seguinte")

![Git Windows](/images/oci-okit-linux-windows/win-15.png "Seguinte")

![Git Windows](/images/oci-okit-linux-windows/win-16.png "Seguinte")

![Git Windows](/images/oci-okit-linux-windows/win-17.png "Seguinte")

![Git Windows](/images/oci-okit-linux-windows/win-18.png "Seguinte")

![Git Windows](/images/oci-okit-linux-windows/win-19.png "Instalar")

![Git Windows](/images/oci-okit-linux-windows/win-20.png "A Finalizar instalación")

Abrimos um CMD de Windows ou PowerShell e verificamos a versão do Git instalado:  

```cmd
Microsoft Windows [Versión 10.0.19041.423]
(c) 2020 Microsoft Corporation. Todos los derechos reservados.

C:\Users\usuario>git --version
git version 2.28.0.windows.1

C:\Users\usuario>
```

#### Instalar o Docker

***

Baixamos o instalador do Docker Desktop:  

- 64 bits <https://download.docker.com/win/stable/Docker%20Desktop%20Installer.exe>

Abrimos o instalador com privilegios de Administrador e vamos instalar o Docker Desktop com as opções padrão:  

![Docker](/images/oci-okit-linux-windows/win-21.png "Deixamos habilitadas ambas opções, ativar WSL v2 e permitimos colocar um acesso direito na Área de Trabalho")

![Docker](/images/oci-okit-linux-windows/win-22.png "A Instalar")

![Docker](/images/oci-okit-linux-windows/win-23.png "Reiniciamos o sistema novamente")

![Docker](/images/oci-okit-linux-windows/win-24.png "Reiniciamos mais uma vez, veremos o icone de Docker na barra de tarefas")

## Instalar Okit Designer

***

### Clonamos o repositório do Okit

***

Clonamos o repositório oficial do Okit Designer (a versão atual é a 0.9.2):  

```bash
$ git clone -b v0.9.2 --depth 1 https://github.com/oracle/oci-designer-toolkit.git
```

### A Criar o arquivo de configuração do Okit e a chave ssh para nos conectar à API do OCI

***

No caso de não ter instalado e configuado a oci-cli (será tema para outro post), deveremos criar um arquvo de configuração que terá contenido o id da conta, além da chave SSH para poder acessar à API e fazer deploy do que tinhamos desenhado:  

Criamos a pasta containers/oci dentro da pasta oci-designer-toolkit: 

**Fedora 32:**  

```bash
$ mkdir /home/usuario/oci-designer-toolkit/containers/oci
$ cd /home/usuario/oci-designer-toolkit/containers/oci
```

**Windows 10 Pro:**  

```poweshell
PS D:\Users\usuario>mkdir D:\Users\usuario\oci-designer-toolkit\containers\oci
PS D:\Users\usuario>cd D:\Users\usuario\oci-designer-toolkit\containers\oci
```

Geramos a chave ssh, vamos colocar a localização aonde queremos salvar o arquivo e como formato .pem (No Windows podemos deixar a localização padrão (pasta .ssh na pasta home do usuario) e logo copia-lâ à nossa pasta containers\oci):  

```bash
$ ssh-keygen -t rsa -b 4096 -m PEM
Generating public/private rsa key pair.
Enter file in which to save the key (/home/usuario/.ssh/id_rsa): /home/usuario/oci-designer-toolkit/containers/oci/oci_api_key.pem
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/usuario/oci-designer-toolkit/containers/oci/oci_api_key.pem
Your public key has been saved in /home/usuario/oci-designer-toolkit/containers/oci/oci_api_key.pem.pub
The key fingerprint is:
SHA256:Dg4RrcxIYUREOcvrEKZ7mL8ZcqRnmkibkXDOQVhZelE usuario@localhost.localdomain
The key's randomart image is:
+---[RSA 4096]----+
| =O=+oE          |
| +=. o.          |
|.oo*o.           |
|.o+.+.           |
|+.+.. . S        |
|+*o. o o         |
|oXB   . .        |
|*B*o             |
|+=+.             |
+----[SHA256]-----+
```

Criamos o arquivo de configuração em /home/usuario/oci-designer-toolkit/containers/oci/config, com o seu editor de textos preferido:  

**Fedora 32:**  

```bash
$ vim /home/usuario/oci-designer-toolkit/containers/oci/config
```

No **Windows 10 Pro**, abrimos o Notepad e vamos escrever os seguintes parámetros:

```plaintext
[DEFAULT]
user=ocid1.user.oc1..aaaaaaaak6z......
fingerprint=3b:7e:37:ec:a0:86:1....
key_file=/home/usuario/oci-designer-toolkit/containers/oci/oci_api_key.pem
tenancy=ocid1.tenancy.oc1..aaaaaaaawpqblfem........
region=us-phoenix-1
```

Onde:  

- **user** é o identificador OCID do usuário no OCI
- **fingerprint** é a  la impressâo digital da chave ssh
- **key_file** é onde se encontra localizada a chave privada ssh (a salvamos em **/home/usuario/oci-designer-toolkit/containers/oci/oci_api_key.pem**)
- **tenancy** é o identificador OCID da tua conta
- **region** é a região padrão donde vais desployar a infraestrutura

Agora bem, ¿Onde conseguimos toda essa informação? Vamos ver as seguintes imagens:.  

![OCI Cuenta](/images/oci-okit-linux-windows/oci-0.png "Colocamos o nome da nossa Conta")

![OCI Login](/images/oci-okit-linux-windows/oci-1.png "Login no website da OCI.")

![Pantalla Principal](/images/oci-okit-linux-windows/oci-2.png "Vamos ao icone de Perfil e fazemos click no nosso usuário cadastrado.")

![Ecrã Usuarios](/images/oci-okit-linux-windows/oci-3.png "Copiamos a nossa OCID de usuário e a colocamos no parámetro user.")

![Ecrã Usuarios](/images/oci-okit-linux-windows/oci-4.png "Vamos à opção Tenancy.")

![Ecrã Usuarios](/images/oci-okit-linux-windows/oci-5.png "Copiamos nuestra OCID de la cuenta y la colocamos en el parámetro tenancy.")

![Ecrã Usuarios](/images/oci-okit-linux-windows/oci-6.png "Vamos de novo ao ecrã usuários, opção API Keys, e adicionamos uma nova.")

![API Key](/images/oci-okit-linux-windows/oci-7.png "Colamos só a CHAVE PÚBLICA, e click em Add.")

![Ecrã Usuarios](/images/oci-okit-linux-windows/oci-8.png "Copiamos o fingerprint, e o colamos no parámetro fingerprint.")

Vamos à pasta e executamos docker-compose para baixar as imagens e contruir o container:  

**Fedora 32:**  

```bash
$ cd /home/usuario/oci-designer-toolkit/containers/docker
$ docker-compose up
```

**Windows 10 Pro:**  

```powershell
PS D:\Users\usuario> cd .\oci-designer-toolkit\containers\docker
PS D:\Users\usuario\oci-designer-toolkit\containers\docker>docker-compose up
```

A Depender da velocidade do nosso sistema, se pode demorar entre 5 a 15 minutos em construir e executar o container.  

```plaintext
Building okit
Step 1/12 : FROM oraclelinux:7-slim
7-slim: Pulling from library/oraclelinux
bce8f778fef0: Pull complete
Digest: sha256:c3150c65fd1e7a13f40599bfadf7d22026e4f0773d1b916c67c27415c4920056
Status: Downloaded newer image for oraclelinux:7-slim
 ---> 153f8d73287e
Step 2/12 : LABEL "provider"="Oracle"       "issues"="https://github.com/oracle/oci-designer-toolkit/issues"       "version"="0.9.2"       "description"="OKIT Web Server Container."       "copyright"="Copyright (c) 2020, Oracle and/or its affiliates."
 ---> Running in 27175738afd7
Removing intermediate container 27175738afd7
 ---> 35e455784ae1
Step 3/12 : SHELL ["/bin/bash", "-c"]
 ---> Running in 629491eb218a
Removing intermediate container 629491eb218a
 ---> c2764521d5de
Step 4/12 : ENV PYTHONIOENCODING=utf8     PYTHONPATH=":/okit/visualiser:/okit/okitweb:/okit"     FLASK_APP=okitweb     FLASK_DEBUG=1     LANG=en_GB.UTF-8     LANGUAGE=en_GB:en     LC_ALL=en_GB.UTF-8     PATH=/root/bin:${PATH}
 ---> Running in c16ff2768ecd
Removing intermediate container c16ff2768ecd
 ---> 39d9d413e910
Step 5/12 : EXPOSE 80
 ---> Running in d0c415984690
Removing intermediate container d0c415984690
 ---> 2f257b72a7f5
Step 6/12 : EXPOSE 443
 ---> Running in 19f827338ecd
Removing intermediate container 19f827338ecd
 ---> 700358c2dc5c
Step 7/12 : RUN yum install -y     oracle-softwarecollection-release-el7     oraclelinux-developer-release-el7  && yum update -y  && yum install -y         python36         python3-pip  && rm -rf /var/cache/yum  && python3 -m pip install --upgrade pip==20.0.2  && pip3 install --no-cache-dir         flask==1.1.1         gunicorn==20.0.4         oci==2.6.0         pyyaml==5.2  && mkdir -p /okit/{config,log,visualiser,okitweb,workspace}
 ---> Running in cfba62941d29
Loaded plugins: ovl
Resolving Dependencies
--> Running transaction check
---> Package oracle-softwarecollection-release-el7.x86_64 0:1.0-3.el7 will be installed
---> Package oraclelinux-developer-release-el7.x86_64 0:1.0-5.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

...  -- SAÍDA OMITIDA -- ...

Successfully built 047213fa08c0
Successfully tagged okit:latest
WARNING: Image for service okit was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Creating okit ... done
Attaching to okit
okit    | [2020-08-07 21:47:47 +0000] [1] [INFO] Starting gunicorn 20.0.4
okit    | [2020-08-07 21:47:47 +0000] [1] [INFO] Listening at: http://0.0.0.0:80 (1)
okit    | [2020-08-07 21:47:47 +0000] [1] [INFO] Using worker: sync
okit    | [2020-08-07 21:47:47 +0000] [8] [INFO] Booting worker with pid: 8
okit    | [2020-08-07 21:47:47 +0000] [9] [INFO] Booting worker with pid: 9
okit    | [2020-08-07 21:47:47 +0000] [12] [INFO] Booting worker with pid: 12
okit    | [2020-08-07 21:47:47 +0000] [13] [INFO] Booting worker with pid: 13
```

Só nos resta ir ao link <http://localhost/okit/designer> desde o nosso navegador para acceder à ferramenta.  

![Ecrã Principal](/images/oci-okit-linux-windows/okit-main.png "Ecrã Principal")

Se fizeremos click no menu de sandwich, poderemos ver as opções disponíveis.  

![Okit Opções](/images/oci-okit-linux-windows/okit-options.png "Ecrã Opções")

Espero que tinham gostado deste tutorial, até a próxima!
