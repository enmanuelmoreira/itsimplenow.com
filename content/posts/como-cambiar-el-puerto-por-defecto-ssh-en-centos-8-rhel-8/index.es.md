---
title: "Como cambiar el puerto por defecto de SSH en CentOS 8 / RHEL 8"
date: 2020-08-15
lastmod: 2020-08-15
author: Enmanuel Moreira
description: "En esta guía vamos a cambiar el puerto por defecto del servicio SSH en CentOS 8 / RHEL 8."
draft: true

resources:
- name: "featured-image"
  src: "featured-image.jpg"

categories: ["Seguridad"]
tags: ["redhat","centos","seguridad"]

lightgallery: true
---

<!--more-->

Hola chiquillos!

En esta guía vamos a cambiar el puerto por defecto del servicio SSH en CentOS 8 / RHEL 8.

SSH provee un canal seguro sobre un red insegura usando una architectura de cliente/servidor, conectando un cliente SSH a un servidor SSH. El puerto por defecto es el 22 en TCP, generalmente usado en sistemas basados en Unix/Linux, así como también en Microsoft Windows.

Es una buena práctica cambiar el puerto por defecto, ya que SSH es el punto de entrada favorito de los atacantes. A pesar de poder cambiarlo, no es garantía que no lo encuentren, debido a que hay escaner de puertos que pueden listar y encontrar los puertos abiertos de un servidor. Sin embargo, no se la vamos a poner tan fácil.  

## Comprobando el estatus del servidor SSH

***

Comprobamos el estado actual del servicio SSH con el comando systemctl:

```bash
[root@centos82 ~]# systemctl status sshd
```

Obtendremos la siguiente salida:  

```bash
● sshd.service - OpenSSH server daemon
   Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: enabled)
   Active: active (running) since Sat 2020-08-15 11:46:42 -04; 13min ago
     Docs: man:sshd(8)
           man:sshd_config(5)
 Main PID: 981 (sshd)
    Tasks: 1 (limit: 23956)
   Memory: 5.2M
   CGroup: /system.slice/sshd.service
           └─981 /usr/sbin/sshd -D -oCiphers=aes256-gcm@openssh.com,chacha20-poly1305@openssh.com,aes256-ctr,aes256-cbc,aes128-gcm@openssh.com,aes128-ctr,aes128-cbc -oMACs=hmac-sha2-256-etm@openssh.com,hmac-sh>

ago 15 11:46:42 centos82.itsimplenow.com systemd[1]: Starting OpenSSH server daemon...
ago 15 11:46:42 centos82.itsimplenow.com sshd[981]: Server listening on 0.0.0.0 port 22.
ago 15 11:46:42 centos82.itsimplenow.com sshd[981]: Server listening on :: port 22.
ago 15 11:46:42 centos82.itsimplenow.com systemd[1]: Started OpenSSH server daemon.
ago 15 11:59:26 centos82.itsimplenow.com sshd[4970]: Accepted password for root from 192.168.20.1 port 49354 ssh2
ago 15 11:59:26 centos82.itsimplenow.com sshd[4970]: pam_unix(sshd:session): session opened for user root by (uid=0)
```

Podemos visualizar que ssh esta escuchando por el puerto 22.  

## Cambiando el puerto por defecto

***

El archivo de configuración del servidor SSH se encuentra ubicado en /etc/ssh/sshd_config, que deberemos modificar con su editor de textos de confianza para cambiar el puerto por defecto:  

```bash
[root@centos82 ~]# vim /etc/ssh/sshd_config
```

Modificamos el parámetro Port:  

```plaintext
#Port 22
```

Y descomentamos y modificamos el puerto:  

```plaintext
Port 2222
```

Guardamos los cambios y salimos del editor.  

## Configurando SELinux para permitir un puerto SSH personalizado

***

Por defecto, SELinux no permite la conexión a SSH por un puerto distinto al predeterminado, sin embargo, podemos configurar SELinux para que acepte el puerto al que hemos cambiado el servidor SSH.  

Necesitaremos el programa semanage, que está contenido en el paquete **policycoreutils-python-utils**, por lo que procederemos a instalarlo:  

```bash
[root@centos82 ~]# dnf install policycoreutils-python-utils
```

Ahora añadimos el puerto que hemos indicado:  

```bash
[root@centos82 ~]# semanage port -a -t ssh_port_t -p tcp 2222
```

## Configurando el Firewall para permitir un puerto SSH personalizado

***

Listamos los puertos del firewall abiertos:

```bash
[root@centos82 ~]# firewall-cmd --list-all
```

```plaintext
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp1s0
  sources:
  services: cockpit dhcpv6-client ssh
  ports:
  protocols:
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

Bloqueamos el puerto por defecto, y permitimos la conexión a SSH por el puerto personalizado:  

```bash
[root@centos82 ~]# firewall-cmd --permanent --remove-service=ssh
success
[root@centos82 ~]# firewall-cmd --permanent --add-port=2222/tcp
success
[root@centos82 ~]# firewall-cmd --reload
success
```

## Reiniciando el servicio SSH y comprobando los cambios

***

Reinciamos el servicio SSH:  

```bash
[root@centos82 ~]# systemctl restart sshd
```

Verificamos nuevamente el estatus del servicio SSH:  

```bash
[root@centos82 ~]# systemctl status sshd
```

```plaintext
● sshd.service - OpenSSH server daemon
   Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: enabled)
   Active: active (running) since Sat 2020-08-15 13:18:23 -04; 1min 14s ago
     Docs: man:sshd(8)
           man:sshd_config(5)
 Main PID: 26576 (sshd)
    Tasks: 1 (limit: 23956)
   Memory: 1.2M
   CGroup: /system.slice/sshd.service
           └─26576 /usr/sbin/sshd -D -oCiphers=aes256-gcm@openssh.com,chacha20-poly1305@openssh.com,aes256-ctr,aes>

ago 15 13:18:23 centos82.itsimplenow.com systemd[1]: Starting OpenSSH server daemon...
ago 15 13:18:23 centos82.itsimplenow.com sshd[26576]: Server listening on 0.0.0.0 port 2222.
ago 15 13:18:23 centos82.itsimplenow.com sshd[26576]: Server listening on :: port 2222.
ago 15 13:18:23 centos82.itsimplenow.com systemd[1]: Started OpenSSH server daemon.

```

## Accediendo al servidor SSH con los cambios realizados

***

Intentamos acceder por el puerto 22 al servidor SSH y comprobar si efectivamente está bloqueado:  

```bash
$ ssh root@centos82.itsimplenow.com
ssh: connect to host centos82.itsimplenow.com port 22: Connection refused
```

```bash
$ sftp root@centos82.itsimplenow.com
ssh: connect to host centos82.itsimplenow.com port 22: Connection refused
Connection closed.
Connection closed
```

Nos conectamos a SSH por el nuevo puerto, tenemos que indicar el operador -p para indicar el puerto:  

```bash
$ ssh root@centos82.itsimplenow.com -p 2222
The authenticity of host '[centos82.itsimplenow.com]:2222 ([centos82.itsimplenow.com]:2222)' can't be established.
ECDSA key fingerprint is SHA256:4MDhGEIJQAbfnFPlrTwPOJrcYaAmARmsN8xAlWAb6u0.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[centos82.itsimplenow.com]:2222' (ECDSA) to the list of known hosts.
root@centos82.itsimplenow.com's password:
Activate the web console with: systemctl enable --now cockpit.socket

Last login: Sat Aug 15 13:26:22 2020 from 192.168.20.1
```

Probamos sftp:  

```bash
$ sftp -P 2222 root@centos82.itsimplenow.com
root@centos82.itsimplenow.com's password:
Connected to root@centos82.itsimplenow.com.
sftp>
```

Espero les haya gustado este tutorial, ¡hasta la próxima!
