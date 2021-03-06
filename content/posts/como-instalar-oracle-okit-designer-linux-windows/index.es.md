---
title: "Como Instalar Okit Designer Tookit en Linux y Windows para diseñar diagramas de Oracle Cloud Infrastructure"
date: 2020-08-07
lastmod: 2021-01-22
author: Enmanuel Moreira
description: "En esta guía les voy a explicar como instalar la herramienta Okit Designer Tookit para diseñar nuestros propios diagramas de Oracle Cloud Infrastructure."
draft: false

resources:
- name: "featured-image"
  src: "featured-image.png"

categories: ["Linux"]
tags: ["oracle","oci","contenedores","cloud","redhat","linux","centos","tutoriales","windows","oci-cli","oci-designer"]

lightgallery: true
---

<!--more-->

¡Hola a todos!

En esta guía les voy a explicar como instalar la herramienta Okit Designer Tookit para diseñar nuestros propios diagramas de Oracle Cloud Infrastructure.

## Okit Designer

***

[OCI designer toolKIT (OKIT)](https://github.com/oracle/oci-designer-toolkit) es un conjunto de herramientas que permiten diseñar, desplegar y visualizar ambientes de OCI a través de una interfaz web, lo que permite a arquitectos y diseñadores cloud contruir una representación visual de su infraestructura y luego exportarla en los siguientes formatos:  

- svg
- png
- jpeg  

Una vez que se haya completado el diseño, este se puede exportar a las siguientes herramientas:  

- Ansible
- Terraform
- OCI Resource Manager  

Esto acelera el despliegue de una infraestructura completa en OCI.  

Hay dos maneras de instalar Okit Designer: con Vagrant y Docker. En esta oportunidad lo vamos a hacer por Docker y Docker Compose para aislarlo de nuestro sistema.  

## Instalando Prerrequisitos

***

### Fedora 33

***

Antes de instalar Docker, vamos a actualizar nuestro sistema:  

```bash
sudo dnf update -y
```

En caso que se haya actualizado a un nuevo kernel, reiniciamos el sistema:  

```bash
sudo reboot
```

Luego, Instalamos el paquete:  

```bash
sudo dnf -y install dnf-plugins-core
```

#### Instalando Git

***

```bash
sudo dnf -y install git
```

#### Instalando Docker

***

Lo puedes instalar siguiendo este tutorial: [Como Instalar Docker en Linux](https://www.itsimplenow.com/como-instalar-docker-en-linux/)  

#### Instalando Docker Compose

***

La versión actual es la 1.28.0, bajamos el ejecutable de Docker Compose y lo guardamos en el directorio /usr/local/bin:  

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.28.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

Le asignamos permisos de ejecución:  

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

Comprobamos si está correctamente instalado:  

```bash
docker-compose --version
```

```plaintext
docker-compose version 1.26.2, build eefe0d31
```

### Microsoft Windows 10 Pro

***

#### Activando Componentes necesarios  

***

En el menú Inicio tecleamos "Activar o Desactivar Caracteristicas de Windows":  

![Inicio](/images/oci-okit-linux-windows/win-0.png "Menú Inicio")

Activamos los siguientes componentes:  

- Contenedores
- Hyper-V
- Subsistema de Windows para Linux

![Inicio](/images/oci-okit-linux-windows/win-1.png "Activar o Desactivar Caracteristicas")

Esperamos que se instalen los componentes requeridos:  

![Inicio](/images/oci-okit-linux-windows/win-2.png "Instalando caracteristicas")

Reiniciamos el sistema:  

![Inicio](/images/oci-okit-linux-windows/win-3.png "Reiniciar")

Una vez que hayamos reiniciado el sistema, instalaremos WSL v2.

#### Instalando WSL v2  

***

Descargamos el instalador de WSL v2:  

<https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi>

Abrimos el instalador con permisos de administrador y seguimos los pasos:  

![WSLv2](/images/oci-okit-linux-windows/win-4.png "Abriendo Instalador")

![WSLv2](/images/oci-okit-linux-windows/win-5.png "Finalizando instalación")

Ahora, vamos a configurar para que WSL v2 sea la versión por defecto en nuestro sistema. Para ello debemos abrir una ventana de PoweShell como Administrador (yo uso la versión 7 que puedes descargar [aquí](https://github.com/PowerShell/PowerShell/releases/download/v7.0.3/PowerShell-7.0.3-win-x64.msi)) y colocamos el siguiente comando:

```powershell
wsl --set-default-version 2
```

Ya con esto terminamos de configurar WSL en nuestro sistema.  

#### Instalando Git

***

Descargamos el instalador de Git:  

- 64 bits: <https://github.com/git-for-windows/git/releases/download/v2.28.0.windows.1/Git-2.28.0-64-bit.exe>

Ejecutamos el instalador con permisos de Administrador y vamos a instalar con las opciones por defecto:

![Git Windows](/images/oci-okit-linux-windows/win-6.png "Licencia")

![Git Windows](/images/oci-okit-linux-windows/win-7.png "Directorio Destino, lo dejamos en la ubicación predeterminada")

![Git Windows](/images/oci-okit-linux-windows/win-8.png "Seleccionamos los componentes a instalar, dejaremos los que estan por defecto")

![Git Windows](/images/oci-okit-linux-windows/win-9.png "Iconos del Menú Inicio")

![Git Windows](/images/oci-okit-linux-windows/win-10.png "Dejamos por defecto o podemos cambiar el editor de textos predeterminado")

![Git Windows](/images/oci-okit-linux-windows/win-11.png "Dejamos la opción recomendada")

![Git Windows](/images/oci-okit-linux-windows/win-12.png "Dejamos el ejecutable de ssh que viene con el instalador, en caso de tener ya instalado putty, podemos escoger la segunda opción")

![Git Windows](/images/oci-okit-linux-windows/win-13.png "Dejamos por defecto a OpenSSL para emitir certificados X509")

![Git Windows](/images/oci-okit-linux-windows/win-14.png "Siguiente")

![Git Windows](/images/oci-okit-linux-windows/win-15.png "Siguiente")

![Git Windows](/images/oci-okit-linux-windows/win-16.png "Siguiente")

![Git Windows](/images/oci-okit-linux-windows/win-17.png "Siguiente")

![Git Windows](/images/oci-okit-linux-windows/win-18.png "Siguiente")

![Git Windows](/images/oci-okit-linux-windows/win-19.png "Instalar")

![Git Windows](/images/oci-okit-linux-windows/win-20.png "Finalizando instalación")

Vamos al CMD de Windows o PowerShell y verificamos la versión de Git instalada:  

```cmd
Microsoft Windows [Versión 10.0.19041.423]
(c) 2020 Microsoft Corporation. Todos los derechos reservados.

C:\Users\usuario>git --version
git version 2.28.0.windows.1

C:\Users\usuario>
```

#### Instalando Docker

***

Descargamos el instalador de Docker Desktop:  

- 64 bits <https://download.docker.com/win/stable/Docker%20Desktop%20Installer.exe>

Ejecutamos el instalador con permisos de Administrador y vamos a instalar con las opciones por defecto:  

![Docker](/images/oci-okit-linux-windows/win-21.png "Dejamos habilitadas ambas opciones, activar WSL v2 y que coloque un acceso directo en el escritorio")

![Docker](/images/oci-okit-linux-windows/win-22.png "Instalando")

![Docker](/images/oci-okit-linux-windows/win-23.png "Reiniciamos el sistema nuevamente")

![Docker](/images/oci-okit-linux-windows/win-24.png "Una vez reiniciamos, lo veremos en la barra de tareas")

## Instalando Okit Designer

***

### Clonamos el repositorio de Okit

***

Clonamos el repositorio oficial de Okit Designer (la versión actual es la 0.9.2):  

```bash
git clone -b v0.9.2 --depth 1 https://github.com/oracle/oci-designer-toolkit.git
```

### Creando archivo de configuración de Okit y la llave ssh para conectarnos a la API de OCI

***

Si no tenemos instalado y configurado la oci-cli (explicado en este [POST](https://itsimplenow.com/como-instalar-oracle-cloud-infrastructure-cli-en-linux-windows)), debemos crear un archivo de configuración que contendrá el id de la cuenta, además de la llave SSH para poder acceder a la API y desplegar lo que ya hayamos diseñado:  

Creamos el directorio containers/oci dentro del directorio raíz (oci-designer-toolkit) ; 

**Fedora 33:**  

```bash
mkdir /home/usuario/oci-designer-toolkit/containers/oci
cd /home/usuario/oci-designer-toolkit/containers/oci
```

**Windows 10 Pro:**  

```poweshell
PS D:\Users\usuario>mkdir D:\Users\usuario\oci-designer-toolkit\containers\oci
PS D:\Users\usuario>cd D:\Users\usuario\oci-designer-toolkit\containers\oci
```

Generamos la llave ssh, colocando la ubicación donde queremos guardar el archivo y el formato .pem (En Windows podemos dejarla en la ubicación predeterminada (directorio .ssh en la carpeta home del usuario) y luego copiarla a nuestra carpeta containers\oci):  

```bash
ssh-keygen -t rsa -b 4096 -m PEM
```

```plaintext
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

Creamos el archivo de configuración en /home/usuario/oci-designer-toolkit/containers/oci/config, con su editor de textos de confianza:  

**Fedora 33:**  

```bash
vim /home/usuario/oci-designer-toolkit/containers/oci/config
```

En **Windows 10 Pro**, abrimos un Notepad y vamos a escribir los siguientes parámetros:

```plaintext
[DEFAULT]
user=ocid1.user.oc1..aaaaaaaak6z......
fingerprint=3b:7e:37:ec:a0:86:1....
key_file=/home/usuario/oci-designer-toolkit/containers/oci/oci_api_key.pem
tenancy=ocid1.tenancy.oc1..aaaaaaaawpqblfem........
region=us-phoenix-1
```

Donde:  

- **user** es el identificador OCID de usuario en OCI
- **fingerprint** es la huella digital de la llave ssh
- **key_file** es la ubicación donde está almacenada la llave privada ssh (la guardamos en **/home/usuario/oci-designer-toolkit/containers/oci/oci_api_key.pem**)
- **tenancy** es el identificador OCID de tu cuenta
- **region** es la región por defecto donde despliegues la infraestructura

Ahora, ¿De donde sacamos toda esta información? En las siguientes imagenes lo vamos a ver.  

![OCI Cuenta](/images/oci-okit-linux-windows/oci-0.png "Colocamos el nombre de nuestra Cuenta")

![OCI Login](/images/oci-okit-linux-windows/oci-1.png "Iniciamos sesión en la página de OCI.")

![Pantalla Principal](/images/oci-okit-linux-windows/oci-2.png "Vamos al icono de Perfil y hacemos click en nuestro usuario registrado.")

![Pantalla Usuarios](/images/oci-okit-linux-windows/oci-3.png "Copiamos nuestra OCID de usuario y la colocamos en el parámetro user.")

![Pantalla Usuarios](/images/oci-okit-linux-windows/oci-4.png "Vamos a la opción Tenancy.")

![Pantalla Usuarios](/images/oci-okit-linux-windows/oci-5.png "Copiamos nuestra OCID de la cuenta y la colocamos en el parámetro tenancy.")

![Pantalla Usuarios](/images/oci-okit-linux-windows/oci-6.png "Vamos de nuevo a la pantalla usuarios, opción API Keys, y agregamos una nueva.")

![API Key](/images/oci-okit-linux-windows/oci-7.png "Arrastramos solo la LLAVE PÚBLICA, y click en Add.")

![Pantalla Usuarios](/images/oci-okit-linux-windows/oci-8.png "Copiamos el fingerprint, y lo pegamos en el parámetro fingerprint.")

Vamos al directorio y ejecutamos docker-compose para descargar las imagenes y contruir el contenedor:  

**Fedora 33:**  

```bash
cd /home/usuario/oci-designer-toolkit/containers/docker
docker-compose up
```

**Windows 10 Pro:**  

```powershell
PS D:\Users\usuario> cd .\oci-designer-toolkit\containers\docker
PS D:\Users\usuario\oci-designer-toolkit\containers\docker>docker-compose up
```

Dependiendo de la velocidad de nuestro sistema, se puede tardar entre 5 a 15 minutos en construir y ejecutar el contenedor.  

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

...  -- SALIDA OMITIDA -- ...

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

Solo nos resta ir al link <http://localhost/okit/designer> desde nuestro navegador para acceder a la herramienta.  

![Pantalla Principal](/images/oci-okit-linux-windows/okit-main.png "Pantalla Principal")

Si hacemos click en el menú de sandwich, veremos las opciones disponibles.  

![Okit Opciones](/images/oci-okit-linux-windows/okit-options.png "Pantalla de Opciones")

Espero les haya gustado este tutorial, ¡hasta la próxima!

## Apoya este Proyecto!!!

Si te pareció útil este artículo y el proyecto en general, considera brindarme un café :)

<style>.bmc-button img{height: 34px !important;width: 35px !important;margin-bottom: 1px !important;box-shadow: none !important;border: none !important;vertical-align: middle !important;}.bmc-button{padding: 7px 15px 7px 10px !important;line-height: 35px !important;height:51px !important;text-decoration: none !important;display:inline-flex !important;color:#ffffff !important;background-color:#5F7FFF !important;border-radius: 8px !important;border: 1px solid transparent !important;font-size: 24px !important;letter-spacing: 0.6px !important;box-shadow: 0px 1px 2px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;margin: 0 auto !important;font-family:'Cookie', cursive !important;-webkit-box-sizing: border-box !important;box-sizing: border-box !important;}.bmc-button:hover, .bmc-button:active, .bmc-button:focus {-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;text-decoration: none !important;box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;opacity: 0.85 !important;color:#ffffff !important;}</style><link href="https://fonts.googleapis.com/css?family=Cookie" rel="stylesheet"><a class="bmc-button" target="_blank" href="https://www.buymeacoffee.com/enmanuelmoreira"><img src="https://cdn.buymeacoffee.com/buttons/bmc-new-btn-logo.svg" alt="Buy me a coffee"><span style="margin-left:5px;font-size:24px !important;">Buy me a coffee</span></a>
