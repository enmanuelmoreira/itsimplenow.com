---
title: "Como Configurar un Cluster de MariaDB con Galera en CentOS 8 / RHEL 8"
date: 2020-08-23
lastmod: 2020-08-23
author: Enmanuel Moreira
description: "Nesta guia vamos mudar a porta padrão de SSH no CentOS 8 / RHEL 8."
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

Nesta guia vamos mudar a porta padrão de SSH no CentOS 8 / RHEL 8.

O serviço SSH fornece um canal seguro sob uma rede insegura usando uma arquitetura cliente/servidor, conetando um cliente SSH ao servidor SSH. A porta padrão é a 22 no protocolo TCP, geralmente usada em sistemas Unix/Linux, como também em Microsoft Windows.

É uma boa prática mudar a porta padrão, já que SSH é o alvo favorito dos atacantes. Apesar de poder troca-lâ, não há garantia nenhuma de não ser encontrada, devido a que há ferramentas que permitem escanear portas abertas em um servidor. Porém, não vamos deixar tão fácil a entrada ao nosso sistema.

## Comprovar o status do servidor SSH

***

Comprovamos o estado atual do serviço SSH com o comando systemctl:

```bash
[root@centos82 ~]# systemctl status sshd
```

Obteremos a seguinte saída:  

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

Vemos que o servidor ssh está a escutar pela porta 22.  

## Mudar a porta padrão

***

O arquivo de configuração do servidor SSH encontra-se encontra localizado em /etc/ssh/sshd_config, vamos modifica-lô com o seu editor de textos preferido e mudar a porta padrão:  

```bash
[root@centos82 ~]# vim /etc/ssh/sshd_config
```

Modificamos o parámetro Port:  

```plaintext
#Port 22
```

Tiramos o # e mudamos a porta:  

```plaintext
Port 2222
```

Salvamos o arquivo e saímos do editor.  

## Configurar SELinux para permitir a porta personalizada

***

Por padrão, o SELinux não permite a conexão pelo SSH a uma porta distinta à porta padrão, porém, podemos configurar o SELinux para permitir a porta que temos mudado.  

Vamos precisar do comando **semanage**, que está no pacote **policycoreutils-python-utils**, no caso de não estar instalando no nosso sistema, vamos instala-lô:  

```bash
[root@centos82 ~]# dnf install policycoreutils-python-utils
```

Agora, vamos anadir a porta que temos mudado:  

```bash
[root@centos82 ~]# semanage port -a -t ssh_port_t -p tcp 2222
```

## Configurar o Firewall e permitir o tráfego pela porta personalizada

***

Vamos consultar as portas do firewall que estajam abertas:

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

Vamos bloquear a porta padrão (22) e vamos permitir o tráfego pela nova porta:  

```bash
[root@centos82 ~]# firewall-cmd --permanent --remove-service=ssh
success
[root@centos82 ~]# firewall-cmd --permanent --add-port=2222/tcp
success
[root@centos82 ~]# firewall-cmd --reload
success
```

## Reiniciar o serviço e comprovar as mudanças feitas

***

Reinciamos o serviço SSH:  

```bash
[root@centos82 ~]# systemctl restart sshd
```

Verificamos novamente o status do serviço SSH:  

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

## Ingresar ao servidor SSH com as novas configurações

***

Tentamos ingresar pela porta 22 e verificar se a porta se encontra efetivamente bloqueada:  

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

Nos conectamos a SSH pela nova porta, teremos que indicar o operador -p para indicar a porta:  

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

Provamos sftp:  

```bash
$ sftp -P 2222 root@centos82.itsimplenow.com
root@centos82.itsimplenow.com's password:
Connected to root@centos82.itsimplenow.com.
sftp>
```

Espero que tinham gostado deste tutorial, até a próxima!
