---
title: "Como Instalar MariaDB no CentOS 8 / RHEL 8"
date: 2020-08-21
lastmod: 2020-08-21
author: Enmanuel Moreira
description: "Nesta guia vamos instalar MariaDB no CentOS 8 / RHEL 8."
draft: true

resources:
- name: "featured-image"
  src: "featured-image.png"

categories: ["Linux", "Banco de Dados"]
tags: ["redhat","centos","mariadb","banco de dados"]

lightgallery: true
---

<!--more-->

Olá Pessoal, tudo bem?

O [MariaDB](https://mariadb.org/) Server, é um dos bancos de dados de código aberto mais popular do mercado. É desenvolvido pelo Michael (Monty) Widenius —fundador da MySQL—, pela fundação MariaDB e pela comunidade de desenvolvedores de software livre. É forcenido pela maior parte dos fornecedores cloud e é padrão em muitas distros Linux.  

Tem compatibilidade com o MySQL já qe possui os mesmos comandos, interfaces, API e livrarias, sendo o seu objetivo poder trocar um server pelo outro direitamente. Introduz dois motores de armazenamento novos: o Aria —que substitui ao MyISAM— e outro chamado XtraDB —que substitui ao InnoDB—.  

Nesta guia vamos instalar MariaDB no CentOS 8 / RHEL 8.

## Instalar o MariaDB Server  

***

Há duas maneiras de instalar o MariaDB: Através do AppStream de CentOS / RHEL ou direito pelos repositórios oficiais do  MariaDB:  

### Instalar pelo AppStream

***

Instalar o MariaDB pelo AppStream é muito fácil, devemos entrar em uma terminal e executar o seguinte comando com privilégios root:  

```bash
[root@centos ~]# dnf install @mariadb
```

### Instalar pelo Repositório da Fundação MariaDB

***

A Fundação MariaDB oferece pacotes precompilados que podem ser instalados através do seu repositório. Se mantém sempre atualizado e com pleno suporte da comunidade.  

Para instala-lôs, devemos incluir os repositórios ao nosso sistema:  

#### CentOS 8

```bash
[root@centos ~]# cat <<EOF >> /etc/yum.repos.d/mariadb.repo
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.4/centos8-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
EOF
```

#### RHEL 8

```bash
[root@centos ~]# cat <<EOF >> /etc/yum.repos.d/mariadb.repo
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.4/rhel8-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
EOF
```

{{< admonition note >}}
Vamos precisar desativar o MariaDB dos repositórios **rhel-8-for-x86_64-appstream-rpms** e **AppStream** de maneira temporal no RHEL 8 e no CentOS 8 respectivamente para permitir ao dnf baixar e instalar os pacotes desde o repositótio oficial do MariaDB.
{{< /admonition >}}

```plaintext
### CentOS 8 ###

dnf install -y boost-program-options
dnf --disablerepo=AppStream install -y MariaDB-server MariaDB-client

### RHEL 8 ###

dnf --disablerepo=rhel-8-for-x86_64-appstream-rpms install -y MariaDB-server MariaDB-client
```

## Iniciar o Serviço MariaDB

***

Uma vez seja instalado o server MariaDB, iniciamos o serviço:  

```bash
[root@centos ~]# systemctl enable --now mariadb
```

Vamos comprobar que se esteja a executar:  

```bash
[root@centos ~]# systemctl status mariadb
```

Devía de mostrar a seguinte saída:  

```plaintext
● mariadb.service - MariaDB 10.4.14 database server
   Loaded: loaded (/usr/lib/systemd/system/mariadb.service; enabled; vendor preset: disabled)
  Drop-In: /etc/systemd/system/mariadb.service.d
           └─migrated-from-my.cnf-settings.conf
   Active: active (running) since Sun 2020-08-21 22:39:54 -04; 1min 32s ago
     Docs: man:mysqld(8)
           https://mariadb.com/kb/en/library/systemd/
  Process: 8034 ExecStartPost=/bin/sh -c systemctl unset-environment _WSREP_START_POSITION (code=exited, status=0/SUCCESS)
  Process: 7993 ExecStartPre=/bin/sh -c [ ! -e /usr/bin/galera_recovery ] && VAR= ||   VAR=`cd /usr/bin/..; /usr/bin/galera_recovery`; [ $? -eq 0 ]   &>
  Process: 7991 ExecStartPre=/bin/sh -c systemctl unset-environment _WSREP_START_POSITION (code=exited, status=0/SUCCESS)
 Main PID: 8003 (mysqld)
   Status: "Taking your SQL requests now..."
    Tasks: 30 (limit: 23980)
   Memory: 75.0M
   CGroup: /system.slice/mariadb.service
           └─8003 /usr/sbin/mysqld

ago 21 22:39:54 centos.itsimplenow.local mysqld[8003]: 2020-08-21 22:39:54 0 [Note] InnoDB: 10.4.14 started; log sequence number 60967; transaction id >
ago 21 22:39:54 centos.itsimplenow.local mysqld[8003]: 2020-08-21 22:39:54 0 [Note] InnoDB: Loading buffer pool(s) from /var/lib/mysql/ib_buffer_pool
ago 21 22:39:54 centos.itsimplenow.local mysqld[8003]: 2020-08-21 22:39:54 0 [Note] InnoDB: Buffer pool(s) load completed at 200823 22:39:54
ago 21 22:39:54 centos.itsimplenow.local mysqld[8003]: 2020-08-21 22:39:54 0 [Note] Plugin 'FEEDBACK' is disabled.
ago 21 22:39:54 centos.itsimplenow.local mysqld[8003]: 2020-08-21 22:39:54 0 [Note] Server socket created on IP: '::'.
ago 21 22:39:54 centos.itsimplenow.local mysqld[8003]: 2020-08-21 22:39:54 0 [Note] Reading of all Master_info entries succeeded
ago 21 22:39:54 centos.itsimplenow.local mysqld[8003]: 2020-08-21 22:39:54 0 [Note] Added new Master_info '' to hash table
ago 21 22:39:54 centos.itsimplenow.local mysqld[8003]: 2020-08-21 22:39:54 0 [Note] /usr/sbin/mysqld: ready for connections.
ago 21 22:39:54 centos.itsimplenow.local mysqld[8003]: Version: '10.4.14-MariaDB'  socket: '/var/lib/mysql/mysql.sock'  port: 3306  MariaDB Server
ago 21 22:39:54 centos.itsimplenow.local systemd[1]: Started MariaDB 10.4.14 database server.
```

## Executar o Assistente de Configuração do Server MariaDB

***

Vamos executar o comando **mysql_secure_installation** para inicializar pela primeira vez o server MariaDB:  

```bash
[root@centos ~]# mysql_secure_installation
```

Respondemos às preguntas do instalador:  

```laintext
NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none): <-- PULSAR ENTER
OK, successfully used password, moving on...

Setting the root password or using the unix_socket ensures that nobody
can log into the MariaDB root user without the proper authorisation.

You already have your root account protected, so you can safely answer 'n'.

Switch to unix_socket authentication [Y/n] N <-- DESATIVAMOS LA AUTENTICACION UNIX
 ... skipping.

You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] Y  <-- MUDAMOS A PALAVRA-CHAVE DO USURIO ROOT
New password: <-- NOVA PALAVRA-CHAVE CONTRASEÑA
Re-enter new password: <-- ESCREVEMOS DE NOVO A PALAVRA-CHAVE
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] Y <-- TIRAMOS O ACESSO AOS USUÁRIOS ANÓNIMOS
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] Y <-- DESATIVAMOS O ACESSO ROOT DE MANEIRA REMOTA«
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] Y <-- APAGAMOS O BANCO DE DADOS DE TESTE
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] Y  <-- ATUALIZAMOS OS PRIVILÉGIOS«
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

## Acessar ao MariaDB

***

O que resta por fazer, é acessar ao server com o seguinte comando:  

```bash
[root@centos ~]# mysql -u root -p
Enter password:
```

Vamos pôr a nossa palavra-chave, e já devíamos de poder acessar ao nosso server  

```bash
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 16
Server version: 10.4.14-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>
```

Espero que tinham gostado deste tutorial, até a próxima!
