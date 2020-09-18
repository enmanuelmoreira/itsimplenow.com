---
title: "Como Instalar Oracle Cloud Infrastructure CLI en Linux y Windows"
date: 2020-08-29
lastmod: 2020-08-29
author: Enmanuel Moreira
description: ""
draft: false

resources:
- name: "featured-image"
  src: "featured-image.jpg"

categories: ["Linux","Cloud","Tutorial","Windows"]
tags: ["linux","oracle","oci","cloud","tutoriales"]

lightgallery: true
---

<!--more-->

¡Hola a todos!

El día de hoy les voy a mostrar como instalar la herramienta CLI de Oracle Cloud Infrastructure para Linux y Windows.  

A diferencia de las herramientas CLI de [AWS](https://docs.aws.amazon.com/es_es/cli/latest/userguide/cli-chap-install.html) o [Azure](https://docs.microsoft.com/es-mx/cli/azure/install-azure-cli?view=azure-cli-latest), la CLI de OCI no cuenta con un instalador oficial que facilitaría mucho la vida, por lo que vamos a meternos de lleno en la instalación "manual" siguiendo la [documentación](https://docs.cloud.oracle.com/es-ww/iaas/Content/API/SDKDocs/climanualinst.htm) de la CLI de Oracle.  

## Requisitos

***

Para poder instalar la CLI de OCI, debemos cumplir con los siguientes requisitos:  

- Tener una cuenta de Oracle Cloud Infrastructure.  
- Un usuario creado en esa cuenta, en un grupo con una política que otorgue los permisos deseados. Este usuario de cuenta puede ser usted, otra persona o un sistema que llama a la API.  
- Un par de claves que se usan para firmar solicitudes de API, con la clave pública cargada en Oracle. Solo el usuario que llama a la API debe poseer la clave privada.  

### Fedora 32 - CentOS 7/8

Necesitaremos tener los siguientes paquetes instalados:  

- Python 3.5+
- gcc
- libffi-devel
- python-devel
- openssl-devel

```bash
$ yum install python3 gcc libffi-devel python3-devel openssl-devel
```

### Microsoft Windows 10

- [Python](https://www.python.org/downloads/) 3.5+
- [Git Bash](https://git-scm.com/download/win) Para poder generar el certificado API
- PowerShell

## Generando la Clave de Firma API

Creamos el directorio .oci, la cual estará en la carpeta home del usuario:  

**En Linux:**

```bash
$ mkdir ~/.oci
```

En **Windows** abrimos una terminal de PowerShell y vamos al directorio del usuario:
```powershell
$ cd C:\Users\usuario
$ mkdir .oci
```

Generamos una llave privada de firma API, colocando la ubicación donde queremos guardar el archivo y el formato .pem, especificando el directorio .oci, la cual usaremos para autenticanos al servicio de OCI:

```bash
$ openssl genrsa -out ~/.oci/oci_api_key.pem 4096
Generating RSA private key, 4096 bit long modulus (2 primes)
....++++
..........................................++++
```

Nos aseguramos que la clave privada la pueda ver solo el usuario:  

```bash
$ chmod 400 ~/.oci/oci_api_key.pem
```

Y ahora generamos la llave pública:  

```bash
$ openssl rsa -pubout -in ~/.oci/oci_api_key.pem -out ~/.oci/oci_api_key_public.pem
writing RSA key
```

Subimos nuestra llave pública, para eso debemos iniciar sesión en nuestra cuenta de OCI, damos click en la esquina superior derecha (el icono de nuestro perfil)  

![Pantalla Usuarios](/images/oci-okit-linux-windows/oci-3.png "Pantalla Usuarios")

Vamos a la pantalla Usuarios, opción API Keys, y agregamos una nueva.  

![Pantalla Usuarios](/images/oci-okit-linux-windows/oci-6.png "Usuarios")

Arrastramos solo la LLAVE PÚBLICA, y click en Add.  

![API Key](/images/oci-okit-linux-windows/oci-7.png "API Keys")

Allí se va a cargar el fingerprint, vamos a copiar ese valor y lo pegamos en un Notepad o en algún editor de textos, que lo vamos a necesitar más adelante.  

![Pantalla Usuarios](/images/oci-okit-linux-windows/oci-8.png "API Keys")

## Instalando Oracle Cloud Infrastructure CLI

***

### Linux

Bajamos e instalamos de una vez la versión más actual del script de instalación:  

```bash
bash -c "$(curl -L https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.sh)"
```

El script nos va a hacer una serie de preguntas:

- ¿En cúal directorio le gustaría instalar? Podemos colocar una ruta alterna, en este caso usaré /home/usuario/oci-cli

```bash
===> In what directory would you like to place the install? (leave blank to use '/home/usuario/lib/oracle-cli'): /opt/oci-cli
-- Creating directory '/home/usuario/oci-cli'.
-- We will install at '/home/usuario/oci-cli'.
```

- ¿En cuál directorio le gustaría instalar el ejecutable oci?

```bash
===> In what directory would you like to place the 'oci' executable? (leave blank to use '/home/usuario/bin'): /home/usuario/oci-cli/
```

- ¿En cuál directorio le gustaría instalar los scripts de OCI?

```bash
===> In what directory would you like to place the OCI scripts? (leave blank to use '/home/usuario/bin/scripts'): /home/usuario/oci-cli/bin/scripts/
```

Nos va a preguntar si queremos instalar paquetes opcionales, presionamos ENTER de momento:  

```bash
===> Currently supported optional packages are: ['db (will install cx_Oracle)']
What optional CLI packages would you like to be installed (comma separated names; press enter if you don't need any optional packages)?: PRESIONAMOS ENTER
```

Para finalizar la configuración nos preguntará si deseamos modificar el PATH en nuestro shell automáticamente, colocamos Y luego ENTER:  

```bash
===> Modify profile to update your $PATH and enable shell/tab completion now? (Y/n): Y
```

Indicamos la ruta de nuestro archivo .bashrc (si usamos bash), o .zshrc (en caso de usar zsh):  

```bash
===> Enter a path to an rc file to update (file will be created if it does not exist) (leave blank to use '/home/usuario/.bashrc'): /home/usuario/.bashrc
-- Backed up '/home/vagrant/.bashrc' to '/home/usuario/.bashrc.backup'
-- Tab completion set up complete.
-- If tab completion is not activated, verify that '/home/usuario/.bashrc' is sourced by your shell.
--
-- ** Run `exec -l $SHELL` to restart your shell. **
--
-- Installation successful.
-- Run the CLI with /home/usuario/oci-cli/oci --help
```

Inyectamos los cambios a nuestro shell:  

bash

```bash
$ source ~/.bashrc
```

zsh 

```bash
$ source ~/.zshrc
```

Comprobamos si está correctamente instalado, consultado la ayuda del programa:  

```bash
$ oci --help
Usage: oci [OPTIONS] COMMAND [ARGS]...

  Oracle Cloud Infrastructure command line interface, with support for
  Audit, Block Volume, Compute, Database, IAM, Load Balancing, Networking,
  DNS, File Storage, Email Delivery and Object Storage Services.

  --- SALIDA OMITIDA ---
```

### Windows 10

- Abrimos una consola de PowerShell como **ADMINISTRADOR**  
- El instalador permite la terminación automática al instalar y ejecutar un script. Para permitir la ejecución de este script, debe activar la política de ejecución RemoteSigned.  

Para configurar la política de ejecución remota para PowerShell, ejecute el siguiente comando:  

```powershell
Set-ExecutionPolicy RemoteSigned
```

- Para ejecutar el script del instalador, ejecute el siguiente comando:  

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.ps1'))"
```

El script nos va a hacer una serie de preguntas:

- ¿En cuál directorio le gustaría instalar? Podemos colocar una ruta alterna, en este caso usaré /home/usuario/oci-cli

```powershell
===> In what directory would you like to place the install? (leave blank to use 'C:\Users\usuario\lib\oracle-cli'): C:\Users\usuario\oracle-cli
-- Creating directory 'C:\Users\usuario\oracle-cli'.
-- We will install at 'C:\Users\usuario\oracle-cli'.
```

- ¿En cuál directorio le gustaría instalar el ejecutable oci?

```powershell
===> In what directory would you like to place the 'oci.exe' executable? (leave blank to use 'C:\Users\usuario\bin'): C:\Users\usuario\oci-cli\
```

- ¿En cuál directorio le gustaría instalar los scripts de OCI?

```powershell
===> In what directory would you like to place the OCI scripts? (leave blank to use 'C:\Users\usuario\oci-cli\bin\scripts'): C:\Users\usuario\oracle-cli\bin\scripts
```

Nos va a preguntar si queremos instalar paquetes opcionales, presionamos ENTER de momento:  

```powershell
===> Currently supported optional packages are: ['db (will install cx_Oracle)']
What optional CLI packages would you like to be installed (comma separated names; press enter if you don't need any optional packages)?: PRESIONAMOS ENTER
```

Para finalizar la configuración nos preguntará si deseamos modificar el PATH en nuestro PowerShell automáticamente, colocamos Y luego ENTER:  

```powershell
===> Modify PATH to include the CLI and enable tab completion in PowerShell now? (Y/n): Y
--
-- ** Close and re-open PowerShell to reload changes to your PATH **
-- In order to run the autocomplete script, you may also need to set your PowerShell execution policy to allow for running local scripts (as an Administrator run Set-ExecutionPolicy RemoteSigned in a PowerShell prompt)
--
-- Installation successful.
-- Run the CLI with C:\Users\usuario\oci-cli\oci.exe --help
VERBOSE: Successfully installed OCI CLI!
```

Cerramos PowerShell y lo volvemos a abrir.  

Comprobamos si está correctamente instalado, consultado la ayuda del programa:  

```powershell
$ oci --help
Usage: oci [OPTIONS] COMMAND [ARGS]...

  Oracle Cloud Infrastructure command line interface, with support for
  Audit, Block Volume, Compute, Database, IAM, Load Balancing, Networking,
  DNS, File Storage, Email Delivery and Object Storage Services.

  --- SALIDA OMITIDA ---
```

## Configurando oci-cli

### Interactivo

Ejecutamos el comando:  

```bash
$ oci setup config
```

Nos realizará unas preguntas, primero por la ubicación del archivo config, dejamos la que nos muestra por defecto:  

```bash
Enter a location for your config [/home/usuario/.oci/config]:
```

Lo siguiente que nos va a pedir es el usuario OCID:  

```bash
Enter a user OCID:
```

Por lo que debemos iniciar sesión en nuestra cuenta OCI, vamos al icono de Perfil y hacemos click en nuestro usuario registrado.

![Pantalla Principal](/images/oci-okit-linux-windows/oci-2.png "Pantalla Principal")

Copiamos nuestra OCID de usuario y la colocamos en el parámetro user.  

![Pantalla Usuarios](/images/oci-okit-linux-windows/oci-3.png "Pantalla Usuarios")

Seguido colocamos el OCID de la cuenta (tenancy):  

```bash
Enter a tenancy OCID:
```

Vamos a la opción Tenancy:  

![Pantalla Usuarios](/images/oci-okit-linux-windows/oci-4.png "Pantalla Usuarios")

Copiamos nuestra OCID de la cuenta y la colocamos en el parámetro tenancy:  

![Pantalla Usuarios](/images/oci-okit-linux-windows/oci-5.png "Pantalla Tenancy")

Luego nos pide escoger la región principal, en mi caso será us-ashburn-1:  

```bash
Enter a region (e.g. ap-chiyoda-1, ap-chuncheon-1, ap-hyderabad-1, ap-melbourne-
1, ap-mumbai-1, ap-osaka-1, ap-seoul-1, ap-sydney-1, ap-tokyo-1, ca-montreal-1,
ca-toronto-1, eu-amsterdam-1, eu-frankfurt-1, eu-zurich-1, me-jeddah-1, sa-saopa
ulo-1, uk-gov-cardiff-1, uk-gov-london-1, uk-london-1, us-ashburn-1, us-gov-ashb
urn-1, us-gov-chicago-1, us-gov-phoenix-1, us-langley-1, us-luke-1, us-phoenix-1
, us-sanjose-1): us-ashburn-1
```

Nos preguntará si deseamos generar una nueva llave o si queremos adjuntar una existente, como ya la creamos al principio del artículo, colocamos "n":  

```bash
Do you want to generate a new API Signing RSA key pair? (If you decline you will
 be asked to supply the path to an existing key.) [Y/n]: n
```

E indicamos la ruta donde se encuentra la clave privada:  

```bash
Enter the location of your API Signing private key file: /home/vagrant/.oci/oci_api_key.pem
Fingerprint: 77:3d:77:73:::::::::::::::::
Config written to /home/usuario/.oci/config
```

### Manual

La manera manual, es crear el archivo de configuración en /home/usuario/.oci/config, con su editor de textos de confianza:  

**Fedora 32:**  

```bash
$ vim /home/usuario/.oci/config
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
- **key_file** es la ubicación donde está almacenada la llave privada ssh (la guardamos en **/home/usuario/.oci/oci_api_key.pem**) En Windows substituimos a la ruta del usuario (normalmente **C:\Users\usuario\.oci\oci_api_key.pem**)
- **tenancy** es el identificador OCID de tu cuenta
- **region** es la región por defecto donde despliegues la infraestructura

Y ya tendremos instalado y configurado el CLI de OCI en nuestro sistema.  

Espero les haya gustado este tutorial, ¡hasta la próxima!
