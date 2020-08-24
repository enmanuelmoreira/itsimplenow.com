---
title: "Como Configurar un Cluster de MariaDB con Galera en CentOS 8 / RHEL 8"
date: 2020-08-23
lastmod: 2020-08-23
author: Enmanuel Moreira
description: "MariaDB es uno de los servidores de base de datos de código abierto más popular. Es desarrollado por Michael (Monty) Widenius —fundador de MySQL—, la fundación MariaDB y la comunidad de desarrolladores de software libre. Es ofrecido por la mayor parte de los proveedores cloud y está por efecto en muchas distribuciones Linux.  "
draft: true

resources:
- name: "featured-image"
  src: "featured-image.png"

categories: ["Linux", "Base de Datos"]
tags: ["redhat","centos","mariadb","base de datos","cluster"]

lightgallery: true
---

<!--more-->

Hola chiquillos!

[MariaDB](https://mariadb.org/) es uno de los servidores de base de datos de código abierto más popular. Es desarrollado por Michael (Monty) Widenius —fundador de MySQL—, la fundación MariaDB y la comunidad de desarrolladores de software libre. Es ofrecido por la mayor parte de los proveedores cloud y está por efecto en muchas distribuciones Linux.  

Tiene una alta compatibilidad con MySQL ya que posee las mismas órdenes, interfaces, API y bibliotecas, siendo su objetivo poder cambiar un servidor por otro directamente. Introduce dos motores de almacenamiento nuevos, uno llamado Aria —que reemplaza a MyISAM— y otro llamado XtraDB —en sustitución de InnoDB—.  

En esta guía vamos a instalar MariaDB en CentOS 8 / RHEL 8.

## Instalando MariaDB Server  

***

Hay dos maneras de instalar MariaDB: A través del AppStream de CentOS / RHEL o directamente desde los repositorios oficiales de MariaDB:  

### Instalar desde AppStream

***

Instalar MariaDB mediante AppStream es muy facil, debemos entrar a una terminal y ejecutar el siguiente comando con privilegios root:  

```bash
[root@centos ~]# dnf install @mariadb
```

### Instalar desde el repositorio de la Fundación MariaDB

***

La Fundación MariaDB ofrece paquetes precompilados que pueden ser instalados a través de su repositorio. Se mantiene siempre actualizado y con soporte de la comunidad.  

Para instalarlos, debemos añadir los repositorios a nuestro sistema:  

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
Necesitaremos deshabilitar MariaDB de los repositorios de **rhel-8-for-x86_64-appstream-rpms** y **AppStream** de manera temporal en RHEL 8 and CentOS 8 respectivamente para permitir a dnf descargar e instalar los paquetes desde el repositorio oficial de MariaDB.
{{< /admonition >}}

```plaintext
### CentOS 8 ###

dnf install -y boost-program-options
dnf --disablerepo=AppStream install -y MariaDB-server MariaDB-client

### RHEL 8 ###

dnf --disablerepo=rhel-8-for-x86_64-appstream-rpms install -y MariaDB-server MariaDB-client
```

## Iniciar el Servicio MariaDB

***

Una vez instalado el servidor MariaDB, iniciamos el servicio:  

```bash
[root@centos ~]# systemctl enable --now mariadb
```

Comprobamos que se esté ejecutando:  

```bash
[root@centos ~]# systemctl status mariadb
```

Nos debería mostrar la siguiente salida:  

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

## Ejecutar el Asistente de Configuración del Servidor MariaDB

***

Ejecutamos el comando **mysql_secure_installation** para inicializar por primera vez el servidor MariaDB:  

```bash
[root@centos ~]# mysql_secure_installation
```

Respodemos las preguntas del instalador:  

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

Switch to unix_socket authentication [Y/n] N <-- DESHABILITAMOS LA AUTENTICACION UNIX
 ... skipping.

You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] Y  <-- CAMBIAMOS LA CONTRASEÑA ROOT
New password: <-- NUEVA CONTRASEÑA
Re-enter new password: <-- REESCRIBIMOS LA NUEVA CONTRASEÑA
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] Y <-- REMOVEMOS EL ACCESO A USUARIOS ANONIMOS
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] Y <-- DESHABILITAMOS EL ACCESO A ROOT DE MANERA REMOTA
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] Y <-- ELIMINAMOS LA BASE DE DATOS TEST
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] Y  <-- RECARGAMOS LOS PRIVILEGIOS
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

## Accesar a MariaDB

***

Lo que nos queda ahora, es accesar al servidor mediante el siguiente comando:  

```bash
[root@centos ~]# mysql -u root -p
Enter password:
```

Colocamos nuestra contraseña, y ya deberiamos poder entrar al servidor.  

```bash
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 16
Server version: 10.4.14-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>
```

Espero les haya gustado este tutorial, ¡hasta la próxima!
