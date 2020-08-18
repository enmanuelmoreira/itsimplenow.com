---
title: "Instalar um cluster local do OpenShift 4.4 com CodeReady Containers"
date: 2020-07-08
lastmod: 2020-07-23
author: Enmanuel Moreira
description: "Neste tutorial vou-lhes explicar como desployar um cluster simples do OpenShift para provas através da aplicação CodeReady Containers."
draft: false

resources:
- name: "featured-image"
  src: "featured-image.png"

categories: ["Containers"]
tags: ["openshift", "Containers", "kubernetes", "devops", "redhat", "centos","tutoriais"]

lightgallery: true
---

<!--more-->

Olá Pessoal, tudo bem?

Neste tutorial vou-lhes explicar como fazer deploy de um cluster simples do OpenShift para provas através da aplicação CodeReady Containers.  

O OpenShift é a solução da Red Hat para a orquestação de conteiners basado em Kubernetes. ¿Te animas a provar a última versão do OpenShift sem ter que instalar um grupo de masters e workers?  

Ao momento desta publicação, teremos instalada a versão 4.4.8 do OpenShift.  

### ATUALIZAÇÃO 23/07/2020: Já está liberada a versâo 4.5.1 do OpenShift, pelo que se segues a realizar os mesmos passos cá descritos, vais poder provar a última versão

### Requisitos Mínimos de Hardware

***

Para poder executar corretamente o OpenShift, vamos precisar dos requisitos mínimos de Hardware a seguir:  

* 4 CPUs virtuales (vCPUs)  
* 8 GB de memória RAM.  
* 35 GB de espaço no disco duro.  

CodeReady Containers pode ser executado quanto no Linux, Windows ou MacOS. Porém, o provei no Fedora 32 e no CentOS 7. CodeReady Contains vem empacotado numa máquina virtual Red Hat Enterprise Linux que usar os hipervisores nativos do Linux (libvirt/KVM), Windows 10 (Hyper-V) e MacOS (HyperKit).  

### Passo 1: Registar-nos no website Developers de Red Hat

***

Para poder baixar o executable de CodeReady Containers, vamos precisar registar-nos no seguinte link: **[Red Hat Developers](https://developers.redhat.com/download-manager/link/3868678)** aliás de obter a nossa chave Secret Pull.    

### Passo 2: Instalar dependências necessárias

***

CodeReady Containers precisa os pacotes **libvirt** e **NetworkManager**, os quais devem ser instalados.  

Fedora 32  

```bash
$ sudo dnf update -y
$ sudo dnf install libvirtd qemu-kvm virt-install NetworkManager
$ sudo systemctl enable --now libvirtd
```

Centos 7  

```bash
$ sudo yum update -y
$ sudo yum install libvirtd qemu-kvm virt-install bridge-utils NetworkManager
$ sudo systemctl enable --now libvirtd
```

### Passo 3: Instalar CodeReady Containers  

***

Baixamos o executable atualizado de CRC desde o seguinte link:  

```bash
$ wget https://mirror.openshift.com/pub/openshift-v4/clients/crc/latest/crc-linux-amd64.tar.xz
```

Extraímos os arquivos:  

```bash
$ tar xvf crc-linux-amd64.tar.xz
```

Copiamos o executable no **$PATH** do usuário:  

```bash
$ cd crc
$ sudo cp crc /usr/local/bin
```

Confirmamos a instalação a validar a versão do software:  

```bash
$ crc version
```

É uma boa prática consutar a Ajuda e qualquer programa, pelo que executamos:  

```bash
$ crc --help
CodeReady Containers is a tool that manages a local OpenShift 4.x cluster optimized for testing and development purposes

Usage:
  crc [flags]
  crc [command]

Available Commands:
  cleanup     Undo config changes
  config      Modify crc configuration
  console     Open the OpenShift Web Console in the default browser
  delete      Delete the OpenShift cluster
  help        Help about any command
  ip          Get IP address of the running OpenShift cluster
  oc-env      Add the 'oc' binary to PATH
  podman-env  Setup podman environment
  setup       Set up prerequisites for the OpenShift cluster
  start       Start the OpenShift cluster
  status      Display status of the OpenShift cluster
  stop        Stop the OpenShift cluster
  version     Print version information

Flags:
  -f, --force              Forcefully perform an action
  -h, --help               help for crc
      --log-level string   log level (e.g. "debug | info | warn | error") (default "info")

Use "crc [command] --help" for more information about a command.
```

### Passo 4: Configurar a máquina virtual de CodeReady Containers

***

Executamos o seguinte comando para configurar a máquina host:

```bash
$ crc setup
```

O instalador vai comprobar os requisitos do sistema antes de fazer o deploy da máquina virtual.  

```bash
INFO Checking if oc binary is cached
INFO Caching oc binary
INFO Checking if podman remote binary is cached
INFO Checking if goodhosts binary is cached
INFO Caching goodhosts binary
INFO Checking if CRC bundle is cached in '$HOME/.crc'
INFO Unpacking bundle from the CRC binary
INFO Checking if running as non-root
INFO Checking if Virtualization is enabled
INFO Checking if KVM is enabled
INFO Checking if libvirt is installed
INFO Checking if user is part of libvirt group
INFO Checking if libvirt is enabled
INFO Checking if libvirt daemon is running
INFO Checking if a supported libvirt version is installed
INFO Checking if crc-driver-libvirt is installed
INFO Installing crc-driver-libvirt
INFO Checking for obsolete crc-driver-libvirt
INFO Checking if libvirt 'crc' network is available
INFO Checking if libvirt 'crc' network is active
INFO Checking if NetworkManager is installed
INFO Checking if NetworkManager service is running
INFO Checking if /etc/NetworkManager/conf.d/crc-nm-dnsmasq.conf exists
INFO Checking if /etc/NetworkManager/dnsmasq.d/crc.conf exists
Setup is complete, you can now run 'crc start' to start the OpenShift cluster
```

Uma vez a que a comprobação finalizar, iniciamos o cluster como o seguinte comando:  

```bash
$ crc start
INFO Checking if oc binary is cached
INFO Checking if podman remote binary is cached
INFO Checking if goodhosts binary is cached
INFO Checking if running as non-root
INFO Checking if Virtualization is enabled
INFO Checking if KVM is enabled
INFO Checking if libvirt is installed
INFO Checking if user is part of libvirt group
INFO Checking if libvirt daemon is running
INFO Checking if a supported libvirt version is installed
INFO Checking if crc-driver-libvirt is installed
INFO Checking if libvirt 'crc' network is available
INFO Checking if libvirt 'crc' network is active
INFO Checking if NetworkManager is installed
INFO Checking if NetworkManager service is running
INFO Checking if /etc/NetworkManager/conf.d/crc-nm-dnsmasq.conf exists
INFO Checking if /etc/NetworkManager/dnsmasq.d/crc.conf exists
? Image pull secret [? for help]
```

Já nesta etapa, vai precisar a chave Pull Secret para continuar com a instalação, a qual pode copiar a baixalâ desde o seguinte link **[Install on Laptop: Red Hat CodeReady Containers](https://cloud.redhat.com/openshift/install/crc/installer-provisioned)**  

Cole a chave Pull Secret na terminal, e a instalação vai continuar (este processo pode demorar alguns minutos a depender do teu computador):  

```bash
INFO Extracting bundle: crc_libvirt_4.4.8.crcbundle ...
INFO Checking size of the disk image /home/enmanuelmoreira/.crc/cache/crc_libvirt_4.4.8/crc.qcow2 ...
INFO Creating CodeReady Containers VM for OpenShift 4.4.8...
INFO CodeReady Containers VM is running
INFO Verifying validity of the cluster certificates ...
INFO Check internal and public DNS query ...
INFO Check DNS query from host ...
INFO Generating new SSH key
INFO Copying kubeconfig file to instance dir ...
INFO Starting OpenShift kubelet service
INFO Configuring cluster for first start
INFO Adding user's pull secret ...
INFO Updating cluster ID ...
INFO Starting OpenShift cluster ... [waiting 3m]
INFO
INFO To access the cluster, first set up your environment by following 'crc oc-env' instructions
INFO Then you can access it by running 'oc login -u developer -p developer https://api.crc.testing:6443'
INFO To login as an admin, run 'oc login -u kubeadmin -p fq66o-KsVBU-cnKBU-xLpqd https://api.crc.testing:6443'
INFO
INFO You can now run 'crc console' and use these credentials to access the OpenShift web console
Started the OpenShift cluster
WARN The cluster might report a degraded or error state. This is expected since several operators have been disabled to lower the resource usage. For more information, please consult the documentation
```

Veremos as as credenciais de acesso nas últimas 6 linhas:  

```bash
INFO Then you can access it by running 'oc login -u developer -p developer https://api.crc.testing:6443'
INFO To login as an admin, run 'oc login -u kubeadmin -p fq66o-KsVBU-cnKBU-xLpqd https://api.crc.testing:6443'
INFO You can now run 'crc console' and use these credentials to access the OpenShift web console
```

Habilitamos o acesso ao cluster, a configurar as variáveis de ambiente:  

```bash

$ crc oc-env
$ export PATH="/home/user/.crc/bin:$PATH"
$ eval $(crc oc-env)

```

Atualizamos a shell para que podamos executar os comandos do OpenShift:  

```bash
bash
$ vim ~/.bashrc
export PATH="~/.crc/bin:$PATH"
eval $(crc oc-env)

zsh
$ vim ~/.zshrc
export PATH="~/.crc/bin:$PATH"
eval $(crc oc-env)
```

E depois source  

```bash
bash
$ source ~/.bashrc

zsh
$ source ~/.zshrc
```

Iniciamos sessão como admin, com o seguinte comando:  

```bash
$ oc login -u kubeadmin -p fq66o-KsVBU-cnKBU-xLpqd https://api.crc.testing:6443
The server uses a certificate signed by an unknown authority.
You can bypass the certificate check, but any data you send to the server could be intercepted by others.
Use insecure connections? (y/n): y

Login successful.

You have access to 57 projects, the list has been suppressed. You can list all projects with 'oc projects'

Using project "default".
```

Confirmamos a configuração do cluster:  

```bash
$ oc cluster-info
Kubernetes master is running at https://api.crc.testing:6443

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

```bash
$ oc get nodes
NAME                 STATUS   ROLES           AGE   VERSION
crc-rtgqw-master-0   Ready    master,worker   19d   v1.17.1+3f6f40d
```

```bash
$ oc config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://192.168.42.229:8443
  name: 192-168-42-229:8443
- cluster:
    insecure-skip-tls-verify: true
    server: https://api.crc.testing:6443
  name: api-crc-testing:6443
- cluster:
    certificate-authority: /home/user/.minikube/ca.crt
    server: https://192.168.39.94:8443
  name: minikube
contexts:
- context:
    cluster: api-crc-testing:6443
    namespace: default
    user: kube:admin
  name: default/api-crc-testing:6443/kube:admin
- context:
    cluster: minikube
    user: minikube
  name: minikube
- context:
    cluster: 192-168-42-229:8443
    namespace: myproject
    user: developer/192-168-42-229:8443
  name: minishift
- context:
    cluster: 192-168-42-229:8443
    namespace: myproject
    user: developer/192-168-42-229:8443
  name: myproject/192-168-42-229:8443/developer
- context:
    cluster: 192-168-42-229:8443
    namespace: myproject
    user: system:admin/192-168-42-229:8443
  name: myproject/192-168-42-229:8443/system:admin
current-context: default/api-crc-testing:6443/kube:admin
kind: Config
preferences: {}
users:
- name: developer/192-168-42-229:8443
  user:
    token: EKQqy2H3FLZ0t05RVWUo1wDt_PbSvINAVDfnWR9hGYg
- name: kube:admin
  user:
    token: kAiAW0DlssvmscV_30vVQtHOtoY68oq6NnmgVWAd0uM
- name: minikube
  user:
    client-certificate: /home/user/.minikube/profiles/minikube/client.crt
    client-key: /home/user/.minikube/profiles/minikube/client.key
- name: system:admin/192-168-42-229:8443
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
```

Para ver os operadores do cluster:  

```bash
$ oc get clusteroperators
NAME                                       VERSION   AVAILABLE   PROGRESSING   DEGRADED   SINCE
authentication                             4.4.8     True        False         False      19d
cloud-credential                           4.4.8     True        False         False      19d
cluster-autoscaler                         4.4.8     True        False         False      19d
console                                    4.4.8     True        False         False      19d
csi-snapshot-controller                    4.4.8     True        False         False      19d
dns                                        4.4.8     True        False         False      19d
etcd                                       4.4.8     True        False         False      19d
image-registry                             4.4.8     True        False         False      19d
ingress                                    4.4.8     True        False         False      19d
insights                                   4.4.8     True        False         False      19d
kube-apiserver                             4.4.8     True        False         False      19d
kube-controller-manager                    4.4.8     True        False         False      19d
kube-scheduler                             4.4.8     True        False         False      19d
kube-storage-version-migrator              4.4.8     True        False         False      19d
machine-api                                4.4.8     True        False         False      19d
machine-config                             4.4.8     True        False         False      19d
marketplace                                4.4.8     True        False         False      44m
monitoring                                 4.4.8     True        False         False      19d
network                                    4.4.8     True        False         False      19d
node-tuning                                4.4.8     True        False         False      19d
openshift-apiserver                        4.4.8     True        False         False      38m
openshift-controller-manager               4.4.8     True        False         False      18d
openshift-samples                          4.4.8     True        False         False      19d
operator-lifecycle-manager                 4.4.8     True        False         False      19d
operator-lifecycle-manager-catalog         4.4.8     True        False         False      19d
operator-lifecycle-manager-packageserver   4.4.8     True        False         False      40m
service-ca                                 4.4.8     True        False         False      19d
service-catalog-apiserver                  4.4.8     True        False         False      19d
service-catalog-controller-manager         4.4.8     True        False         False      19d
storage                                    4.4.8     True        False         False      19d
```

### Passo 5: Acedemos ao cluster

***

Pode se acessar ao cluster localmente pela consola ou pela consola web:

```bash
S oc login -u developer -p developer https://api.crc.testing:6443
The server uses a certificate signed by an unknown authority.
You can bypass the certificate check, but any data you send to the server could be intercepted by others.
Use insecure connections? (y/n): y

Login successful.

You don't have any projects. You can try to create a new project, by running

    oc new-project <projectname>
```

Acessar como administrador:

```bash
$ oc login -u kubeadmin -p fq66o-KsVBU-cnKBU-xLpqd https://api.crc.testing:6443
The server uses a certificate signed by an unknown authority.
You can bypass the certificate check, but any data you send to the server could be intercepted by others.
Use insecure connections? (y/n): y

Login successful.

You have access to 57 projects, the list has been suppressed. You can list all projects with 'oc projects'

Using project "default".
```

Para abrir a consola web desde o seu navegador padrão:  

```bash
$ crc console
```

<!-- Galería del Login a CRC -->
![Login](/images/openshift-crc/crc1.png "Login")
![Iniciar Sessão](/images/openshift-crc/crc2.png "Ecrã Iniciar Sessão")
![OpenShift Home](/images/openshift-crc/crc3.png "Ecrã Principal")

Si queremos saber la dirección IP del cluster:

```bash
$ crc ip
```

### Passo 6: Parar o cluster el cluster do OpenShift

***

Para parar o cluster, executamos o seguinte comando:

```bash
$ crc stop
Stopping the OpenShift cluster, this may take a few minutes...
Stopped the OpenShift cluster
```

Pode ser iniciado novamente simplesmente a executar:

```bash
$ crc start
```

### Passo 7: Apagar o cluster do OpenShift

***

Se queremos apagar o cluster do disco duro:

```bash
$ crc delete
```

Espero que tinham gostado deste tutorial, até a próxima!
