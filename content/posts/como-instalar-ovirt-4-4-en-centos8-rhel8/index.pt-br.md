---
title: "Como instalar oVirt 4.4 en Centos 8 / RHEL 8"
date: 2020-08-04
lastmod: 2020-08-04
author: "Enmanuel Moreira"
description: "Hoje vamos a instalar un servidor Self-hosted (standalone) de Ovirt no Centos 8 / RHEL 8."
draft: false

resources:
- name: "featured-image"
  src: "featured-image.png"

categories: ["Virtualização"]
tags: ["ovirt", "virtualização", "centos", "rhel"]

lightgallery: true
---

<!--more-->

Olá Pessoal, tudo bem?

Hoje vamos a instalar un servidor Self-hosted (standalone) de Ovirt no Centos 8 / RHEL 8.  

### O qué é Ovirt?

oVirt é uma solução livre e de código aberto, usado em sistmas Linux como Fedora, CentOS y RHEL. Vem a ser a alternativa livre do VMware Sphere no Linux. A comunidade oVirt é suportada pela Red Hat e é considerada como um projecto de muita importância dentro do catálogo de produtos da Red Hat, conhecido como Red Hat Enterprise Virtualization (RHEV)  

O website do projecto oVirt é: **www.ovirt.org**  

oVirt consiste de dois componentes principais:  

* oVirt Engine  
* oVirt Node  

O oVirt Engine é a interface gráfica pela web, onde se podem administrar máquinas virtuais, computo, redes e recursos de armazenamento.  

O oVirt Node é um servidor RHEL / CentOS o Fedora, no qual o serviço vdsm atua como Hypervisor (KVM) aonde todas as  máquinas virtuais são criadas.  

Neste post vamos instalar a última versão de oVirt 4.4.0 no CentOS 8.2 / RHEL 8.2 de maneira standalone, é dizer, vamos instalar o Engine e o Node em só um servido com armazenameno local como prova de conceito. Em outro post explicarei como faze-lô em duas máquinas ou mais, com as suas diferentes opções de armazenamento (NFS, GlusterFS, etc)  

Para este, vamos trabalhar com as seguintes configurações:  

Nome FQDN: ovirt.example.com  

IP: 192.168.1.30  

Alteramos o arquivo /etc/hosts (com o seu de textos de confianza), adicionamos o endereço IP do servidor e o nome FQDN, para que fique desta maneira:  

```plaintext
192.168.1.30 engine.example.com engine
```

Aliás, devemos de ter uma entrada no servidor DNS ou utilizar o dnsmasq para que possa resolver o endereço IP.  

### Requisitos de Hardware  

***

| Hardware        |      Minimo      |  Recomendado  |
|:---------------:|:----------------:|:-------------:|
| CPU (con suporte para Intel® 64 ou AMD64 CPU, AMD-V™ ou Intel VT®) |  CPU Dual Core. | CPU Quad Core ou com mais núcleos. |
| Memória |    4 GB de RAM, porém, a mesma não deve estar sendo consumida por processos adicionais. |  16 GB de RAM. |
| Armazenamento | 25 GB de espaço disponível. |    50 GB de espaço disponível. |
| Rede           |  1 NIC de ao menos uma largura de banda de 1 Gbps.  |  1 NIC de ao menos uma lagura de banda de 1 Gbps. |

Com o seguinte comando verificamos se a nossa CPU suporta virtualização:  

```bash
[root@ovirt ~]# grep -E 'svm|vmx' /proc/cpuinfo | grep nx
```

### Passo 1: Actualizamos o Servidor  

***

```bash
[root@ovirt ~]# dnf update -y
```

Uma vez instaladas as atualizações, reiniciamos el servidor (no caso de ter atualizado o Kernel):  

```bash
[root@ovirt ~]# systemctl reboot
```

### Passo 2: Habilitamos os repositórios do oVirt  

***

Os pacotes de software que se precisam para deployar a plataforma oVirt, não se encontram disponíveis por padrão nos repositórios de CentOS /RHEL, portanto, adicionamos o repositório do oVirt:  

```bash
[root@ovirt ~]# dnf install http://resources.ovirt.org/pub/yum-repo/ovirt-release44.rpm -y
```

### Passo 3: Habilitamos os módulos javapackages-tools, pki-deps e PostgreSQL 12  

***

```bash
[root@ovirt ~]# dnf module enable javapackages-tools pki-deps postgresql:12 -y
```

### Passo 4: Instalamos a plataforma oVirt Engine  

***

```bash
[root@ovirt ~]# dnf install ovirt-engine -y
```

### Passo 5: Iniciamos o instalador do oVirt Engine  

***

Executamos pela terminal o comando:

```bash
[root@ovirt ~]# engine-setup --generate-answer=/root/answer.txt
```

Isto vai iniciar o instalador do oVirt Engine, no entretanto, vai fazer uma série de perguntas durante a instalação e salvará as respostas em um arquivo. Por se logo quremos reusa-lâs para automatizar a instalação.  

Respondemos as perguntas do instalador, para escolher a opção padrão pulsamos ENTER, senão, escolhemos entre as distintas opções que tem:  

```console

[ INFO  ] Stage: Initializing
[ INFO  ] Stage: Environment setup
          Configuration files: /etc/ovirt-engine-setup.conf.d/10-packaging-jboss.conf, /etc/ovirt-engine-setup.conf.d/10-packaging.conf
          Log file: /var/log/ovirt-engine/setup/ovirt-engine-setup-20200715145028-j2fmin.log
          Version: otopi-1.9.2 (otopi-1.9.2-1.el8)
[ INFO  ] Stage: Environment packages setup
[ INFO  ] Stage: Programs detection
[ INFO  ] Stage: Environment setup (late)
[ INFO  ] Stage: Environment customization

          --== PRODUCT OPTIONS ==--

          Configure Cinderlib integration (Currently in tech preview) (Yes, No) [No]: No
          Configure Engine on this host (Yes, No) [Yes]: Yes

          Configuring ovirt-provider-ovn also sets the Default cluster's default network provider to ovirt-provider-ovn.
          Non-Default clusters may be configured with an OVN after installation.
          Configure ovirt-provider-ovn (Yes, No) [Yes]: Yes
          Configure WebSocket Proxy on this host (Yes, No) [Yes]: Yes

          * Please note * : Data Warehouse is required for the engine.
          If you choose to not configure it on this host, you have to configure
          it on a remote host, and then configure the engine on this host so
          that it can access the database of the remote Data Warehouse host.
          Configure Data Warehouse on this host (Yes, No) [Yes]: Yes
          Configure Grafana on this host (Yes, No) [Yes]: Yes
          Configure VM Console Proxy on this host (Yes, No) [Yes]: Yes

          --== PACKAGES ==--

[ INFO  ] Checking for product updates...
[ INFO  ] No product updates found

          --== NETWORK CONFIGURATION ==--

          Host fully qualified DNS name of this server [ovirt.example.com]: ENTER
[WARNING] Failed to resolve ovirt.example.com using DNS, it can be resolved only locally

          Setup can automatically configure the firewall on this system.
          Note: automatic configuration of the firewall may overwrite current settings.
          Do you want Setup to configure the firewall? (Yes, No) [Yes]: Yes
[ INFO  ] firewalld will be configured as firewall manager.

          --== DATABASE CONFIGURATION ==--

          Where is the DWH database located? (Local, Remote) [Local]: ENTER

          Setup can configure the local postgresql server automatically for the DWH to run. This may conflict with existing applications.
          Would you like Setup to automatically configure postgresql and create DWH database, or prefer to perform that manually? (Automatic, Manual) [Automatic]:
          Where is the Engine database located? (Local, Remote) [Local]: ENTER

          Setup can configure the local postgresql server automatically for the engine to run. This may conflict with existing applications.
          Would you like Setup to automatically configure postgresql and create Engine database, or prefer to perform that manually? (Automatic, Manual) [Automatic]: ENTER

          --== OVIRT ENGINE CONFIGURATION ==--

          Engine admin password: <--- INGRESAR PASSWORD
          Confirm engine admin password: <--- CONFIRMAR PASSWORD
          Application mode (Virt, Gluster, Both) [Both]: ENTER
          Use default credentials (admin@internal) for ovirt-p provider-ovn (Yes, No) [Yes]: Yes

          --== STORAGE CONFIGURATION ==--

          Default SAN wipe after delete (Yes, No) [No]: No

          --== PKI CONFIGURATION ==--

          Organization name for certificate [example.com]: ENTER

          --== APACHE CONFIGURATION ==--

          Setup can configure the default page of the web server to present the application home page. This may conflict with existing applications.
          Do you wish to set the application as the default page of the web server? (Yes, No) [Yes]: Yes

          Setup can configure apache to use SSL using a certificate issued from the internal CA.
          Do you wish Setup to configure that, or prefer to perform that manually? (Automatic, Manual) [Automatic]: ENTER

          --== SYSTEM CONFIGURATION ==--


          --== MISC CONFIGURATION ==--

          Please choose Data Warehouse sampling scale:
          (1) Basic
          (2) Full
          (1, 2)[1]: 1
          Use Engine admin password as initial Grafana admin password (Yes, No) [Yes]: Yes

          --== END OF CONFIGURATION ==--

          --== CONFIGURATION PREVIEW ==--

          Application mode                        : both
          Default SAN wipe after delete           : False
          Host FQDN                               : ovirt.example.com
          Firewall manager                        : firewalld
          Update Firewall                         : True
          Set up Cinderlib integration            : False
          Configure local Engine database         : True
          Set application as default page         : True
          Configure Apache SSL                    : True
          Engine database host                    : localhost
          Engine database port                    : 5432
          Engine database secured connection      : False
          Engine database host name validation    : False
          Engine database name                    : engine
          Engine database user name               : engine
          Engine installation                     : True
          PKI organization                        : example.com
          Set up ovirt-provider-ovn               : True
          Grafana integration                     : True
          DWH database host                       : localhost
          DWH database port                       : 5432
          DWH database secured connection         : False
          DWH database host name validation       : False
          DWH database name                       : ovirt_engine_history
          Grafana database user name              : ovirt_engine_history_grafana
          Configure WebSocket Proxy               : True
          DWH installation                        : True
          Configure local DWH database            : True
          Configure VMConsole Proxy               : True

          Please confirm installation settings (OK, Cancel) [OK]: OK o ENTER
[ INFO  ] Stage: Transaction setup
[ INFO  ] Stopping engine service
[ INFO  ] Stopping ovirt-fence-kdump-listener service
[ INFO  ] Stopping dwh service
[ INFO  ] Stopping vmconsole-proxy service
[ INFO  ] Stopping websocket-proxy service
[ INFO  ] Stage: Misc configuration (early)
[ INFO  ] Stage: Package installation
[ INFO  ] Stage: Misc configuration
[ INFO  ] Upgrading CA
[ INFO  ] Initializing PostgreSQL
[ INFO  ] Creating PostgreSQL 'engine' database
[ INFO  ] Configuring PostgreSQL
[ INFO  ] Creating PostgreSQL 'ovirt_engine_history' database
[ INFO  ] Configuring PostgreSQL
[ INFO  ] Creating CA: /etc/pki/ovirt-engine/ca.pem
[ INFO  ] Creating CA: /etc/pki/ovirt-engine/qemu-ca.pem
[ INFO  ] Updating OVN SSL configuration
[ INFO  ] Creating/refreshing DWH database schema
[ INFO  ] Setting up ovirt-vmconsole proxy helper PKI artifacts
[ INFO  ] Setting up ovirt-vmconsole SSH PKI artifacts
[ INFO  ] Configuring WebSocket Proxy
[ INFO  ] Creating/refreshing Engine database schema
[ INFO  ] Creating a user for Grafana
[ INFO  ] Creating/refreshing Engine 'internal' domain database schema
[ INFO  ] Creating default mac pool range
[ INFO  ] Adding default OVN provider to database
[ INFO  ] Adding OVN provider secret to database
[ INFO  ] Setting a password for internal user admin
[ INFO  ] Install selinux module /usr/share/ovirt-engine/selinux/ansible-runner-service.cil
[ INFO  ] Generating post install configuration file '/etc/ovirt-engine-setup.conf.d/20-setup-ovirt-post.conf'
[ INFO  ] Stage: Transaction commit
[ INFO  ] Stage: Closing up
[ INFO  ] Starting engine service
[ INFO  ] Starting dwh service
[ INFO  ] Starting Grafana service
[ INFO  ] Restarting ovirt-vmconsole proxy service

          --== SUMMARY ==--

[ INFO  ] Restarting httpd
          Please use the user 'admin@internal' and password specified in order to login
          Web access is enabled at:
              http://ovirt.example.com:80/ovirt-engine
              https://ovirt.example.com:443/ovirt-engine
          Internal CA 15:40:59:64:D5:C9:39:F2:23:89:41:71:2D:DB:03:B4:60:F0:3F:1F
          SSH fingerprint: SHA256:d948EGOxkthqdAHeVcfnZBqQGrThWeXjkIe8wtw1G7A
[WARNING] Less than 16384MB of memory is available
          Web access for grafana is enabled at:
              https://ovirt.example.com/ovirt-engine-grafana/
          Please run the following command on the engine machine ovirt.example.com, for SSO to work:
          systemctl restart ovirt-engine

          --== END OF SUMMARY ==--

[ INFO  ] Stage: Clean up
          Log file is located at /var/log/ovirt-engine/setup/ovirt-engine-setup-20200804130408-e92f29.log
[ INFO  ] Generating answer file '/var/lib/ovirt-engine/setup/answers/20200804130408-setup.conf'
[ INFO  ] Stage: Pre-termination
[ INFO  ] Stage: Termination
[ INFO  ] Execution of setup completed successfully
```

O firewall se configurará automáticamente abrindo as puertass necessárias para a administração da plataforma oVirt Engine e também da consola das maquinas virtuais.  

Accedemos agora à plataforma web, abrimos um navegador e digitamos a URL:  

***[https://ovirt.example.com](https://ovirt.example.com)***

Ahora poderemos ver a interface de administración do oVirt:  

![Ecrã Principal](/images/ovirt-4.4/ovirt-1.png)

Click em Portal de Administração, colocamos usuario: admin e palavra-chave (a que previamente configuramos no setup):  

![Inicio de Sessão](/images/ovirt-4.4/ovirt-2.png)
![Principal](/images/ovirt-4.4/ovirt-3.png)

Até aquí já temos configurado o oVirt Engine, agora vamos configurar passo por passo o Hypervisor.  

### Passo 6: Configurar o oVirt Node  

***

Vamos ao menu Computo -> Centro de Dados  

![Principal](/images/ovirt-4.4/ovirt-4.png)

Criamos um novo centro de dados:  

Click em Aceitar com essa configuração.  

![Principal](/images/ovirt-4.4/ovirt-5.png)

Agora vamos ao menu Computo -> Clusters  

![Principal](/images/ovirt-4.4/ovirt-6.png)

Criamos um novo cluster de dados, trocando o Centro de Dados que criamos anteriormente (no nosso caso **pve**):  

![Principal](/images/ovirt-4.4/ovirt-7.png)

As opções mais importantes para configurar são: Arquitetura da CPU (neste caso x86_64) e Tipo de CPU, como eu tenho um processador AMD, vou escolher a opção AMD EPYC (se tiveres um processador Intel deves de troca-lâ pela familia a que pertença a CPU)  

Agora vamos ao menú Computo -> Hosts para configurar o host local  

![Principal](/images/ovirt-4.4/ovirt-8.png)

Click en Novo.  

![Principal](/images/ovirt-4.4/ovirt-9.png)

Teremos que colocar o nome do cluster aonde vai ficar localizado, neste caso o que temos criado anteriormente (***cluster1***), por estar o Hypervisor no mesmo host onde está instalado o oVirt Engine, devemos de colocar como Nome **localhost** e em Nome de Host **localhost** mesmo. No campo autenticação escolhemos a opção por palavra-chave do usuário root.  

![Principal](/images/ovirt-4.4/ovirt-10.png)

Vai nos indicar essa messagem, click en Aceitar.  

![Principal](/images/ovirt-4.4/ovirt-11.png)

Neste ponto, vão ser instalados todos os pacotes necessários para o correito funcionamento do oVirt Node. Vamos ao Dashboard e fazemos click no icone da campana (esquina superior direita) veremos o progresso da instalação.  

Uma vez finalizada a instalação, o host já se encontraria ativo.  

![Principal](/images/ovirt-4.4/ovirt-12.png)

No caso que o host não se ativasse o que dê o erro  “não operacional”, teremos que revisar que o tipo da CPU escolhida seja o correto para a arquitetura onde está se executando o Hypervisor. Isto acontece quando se escolhe um processador AMD quando trata-se de um Intel o vice-versa.  

Aplicando trobleshooting teriamos que colocar o host em Manutenção, escolhemos o host da lista, e click no botão Administração, Manutenção, quanto a seguinte imagem:  

![Principal](/images/ovirt-4.4/ovirt-13.png)

### Passo 7: Criamos o armazenamento das imagens VM e ISO  

***

Agora vamos criar o Domínio de Armazenamento, previamente temos que criar as pastas iso e data para armazenar tanto os instaladores quanto os discos virtuais. Na consola tecleamos:  

```bash
[root@ovirt ~]# mkdir -p /vz/{iso,data}
```

Ajustamos o dono das pastas, neste caso: usuário vdsm, grupo kvm:  

```bash
[root@ovirt ~]# chown /vz -R vdsm:kvm
```

Comprovamos as mudanças:  

```bash
[root@ovirt ~]# ls -ls /vz
total 8
4 drwxr-xr-x. 2 vdsm kvm 4096 aug 04 00:39 data
4 drwxr-xr-x. 2 vdsm kvm 4096 aug 04 14:39 iso
[root@ovirt ~]#
```

Vamos ao menu Armazenamento -> Dominios  

![Principal](/images/ovirt-4.4/ovirt-14.png)

Criamos o domínio Data.  

![Principal](/images/ovirt-4.4/ovirt-15.png)

Click em Aceitar.  

![Principal](/images/ovirt-4.4/ovirt-16.png)

Fazemos o mesmo com o domínio ISO.  

![Principal](/images/ovirt-4.4/ovirt-17.png)

Já temos configurado o Domínio de armazenamento.  

![Principal](/images/ovirt-4.4/ovirt-18.png)

Com todos estes passos teremos um host totalmente funcional para fazer provas.

Espero que tinham gostado, até a próxima!

Fontes: ***[Installing oVirt as a self-hosted engine using the command line](https://www.ovirt.org/documentation/installing_ovirt_as_a_self-hosted_engine_using_the_command_line/)***
