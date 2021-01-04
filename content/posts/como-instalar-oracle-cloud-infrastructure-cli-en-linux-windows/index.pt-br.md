---
title: "Como Instalar a Oracle Cloud Infrastructure CLI no Linux e no Windows"
date: 2020-08-29
lastmod: 2020-08-29
author: Enmanuel Moreira
description: "Hoje vamos instalar a ferramenta CLI da Oracle Cloud Infrastructure no Linux e Windows."
draft: false

resources:
- name: "featured-image"
  src: "featured-image.jpg"

categories: ["Linux","Cloud","Tutorial"]
tags: ["linux","oracle","oci","cloud"]
lightgallery: true
---

<!--more-->

Olá Pessoal, tudo bem?

Hoje vamos instalar a ferramenta CLI da Oracle Cloud Infrastructure no Linux e Windows.  

Diferente das ferramentas CLI da [AWS](https://docs.aws.amazon.com/es_es/cli/latest/userguide/cli-chap-install.html) ou da [Azure](https://docs.microsoft.com/es-mx/cli/azure/install-azure-cli?view=azure-cli-latest), a CLI da OCI não tem com um instalador "oficial" o qual poderia facilitar muito, pelo qye vamos fazer a instalação "manual" seguindo a [documentação](https://docs.cloud.oracle.com/es-ww/iaas/Content/API/SDKDocs/climanualinst.htm) da CLI de Oracle.  

## Requisitos

***

Para poder instalar a CLI da OCI, devemos cumprir com os seguintes requisitos:  

- Ter uma conta da Oracle Cloud Infrastructure.  
- Criar um usuário nessa conta, em um grupo com uma política que conceder a autorização desejada. Este usuário pode ser vocé mesmo, outra pessoa ou um sistema que faça chamadas à API.  
- Um par de chaves que vão ser usadas para assinar as solicitudes na API, com a chave pública carregada no Oracle. Apenas o usuário que chamará à API deve possuir a chave privada.  

### Fedora 32 - CentOS 7/8

Vamos precisar ter os seguintes pacotes instalados:  

- Python 3.5+
- gcc
- libffi-devel
- python-devel
- openssl-devel

```bash
yum install python3 gcc libffi-devel python3-devel openssl-devel
```

### Microsoft Windows 10

- [Python](https://www.python.org/downloads/) 3.5+
- [Git Bash](https://git-scm.com/download/win) Para poder gerar o certificado API
- PowerShell

## Gerando a Chave de Assinatura API

Criamos a pasta .oci, a qual estará na pasta home do usuario:  

**Linux:**

```bash
mkdir ~/.oci
```

No **Windows** abrimos uma terminal de PowerShell e vamos à pasta do usuario:
```powershell
cd C:\Users\usuario
mkdir .oci
```

Geramos uma chave de assinatura API, colocando a localização onde queremos salvar o arquivo e o formato .pem, especificando a pasta .oci, a qual usaremos para nos autenticar ao serviço da OCI:

```bash
openssl genrsa -out ~/.oci/oci_api_key.pem 4096
```

```bash
Generating RSA private key, 4096 bit long modulus (2 primes)
....++++
..........................................++++
```

Verificamos que a chave privada possa ser vista apenas pelo usuário dono da conta:  

```bash
chmod 400 ~/.oci/oci_api_key.pem
```

E agora geramos a chave pública:  

```bash
openssl rsa -pubout -in ~/.oci/oci_api_key.pem -out ~/.oci/oci_api_key_public.pem
```

Carregamos a nossa chave pública, para isto devemos iniciar sessão em nossa conta da OCI, click no canto superior direito (no icone do nosso perfil)  

![Ecrã Usuários](/images/oci-okit-linux-windows/oci-3.png "Ecrã Usuários")

Vamos à opção Usuários, API Keys, e anadimos uma nova.  

![Ecrã Usuários](/images/oci-okit-linux-windows/oci-6.png "Usuários")

Colamos só a CHAVE PÚBLICA, e click em Add.  

![API Key](/images/oci-okit-linux-windows/oci-7.png "API Keys")

Ali vai se carregar o fingerprint, vamos copiar esse valor e colamos em um Notepad ou em outro editor de textos, já que vamos precisa-lô mais para frente.  

![Ecrã Usuarios](/images/oci-okit-linux-windows/oci-8.png "API Keys")

## Instalando Oracle Cloud Infrastructure CLI

***

### Linux

Descarregamos e instalamos de uma vez a versão mais atualizada do script de instalação:  

```bash
bash -c "$(curl -L https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.sh)"
```

O script vai-nos fazer uma série de perguntas:

- Em qual pasta gostaria instalar? Podemos colocar uma rota alternativa, neste caso vou usar /home/usuario/oci-cli

```bash
===> In what directory would you like to place the install? (leave blank to use '/home/usuario/lib/oracle-cli'): /opt/oci-cli
-- Creating directory '/home/usuario/oci-cli'.
-- We will install at '/home/usuario/oci-cli'.
```

- Em qual pasta gostaria de instalar o executable oci?

```bash
===> In what directory would you like to place the 'oci' executable? (leave blank to use '/home/usuario/bin'): /home/usuario/oci-cli/
```

- Em qual pasta gostaria de instalar os scripts da OCI?

```bash
===> In what directory would you like to place the OCI scripts? (leave blank to use '/home/usuario/bin/scripts'): /home/usuario/oci-cli/bin/scripts/
```

Vai-nos perguntar se quremos instalar pacotes opcionáis, apertamos ENTER pelo momento:  

```bash
===> Currently supported optional packages are: ['db (will install cx_Oracle)']
What optional CLI packages would you like to be installed (comma separated names; press enter if you don't need any optional packages)?: PRESIONAMOS ENTER
```

Para terminar a configuración, vai-nos peguntar se desejamos modificar o PATH do nosso shell, colocamos Y depois ENTER:  

```bash
===> Modify profile to update your $PATH and enable shell/tab completion now? (Y/n): Y
```

Indicamos a rota do nosso arquivo .bashrc (no caso de estar usando bash), ou .zshrc (se usaramos zsh):  

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

Recarregamos a nossa shell:  

bash

```bash
source ~/.bashrc
```

zsh 

```bash
source ~/.zshrc
```

Verificamos se a instalação foi feita corretamente, vamos consultar a ajuda do programa:  

```bash
oci --help
```

```plaintext
Usage: oci [OPTIONS] COMMAND [ARGS]...

  Oracle Cloud Infrastructure command line interface, with support for
  Audit, Block Volume, Compute, Database, IAM, Load Balancing, Networking,
  DNS, File Storage, Email Delivery and Object Storage Services.

  --- SAÍDA OMITIDA ---
```

### Windows 10

- Abrimos uma consola de PowerShell como **ADMINISTRADOR**  
- O instalador permite a execução e término automatico de um script. Para permitir a execução de este, devemos habilitar a política de execução RemoteSigned.  

Para configurar a política de execução remota para PowerShell, execute e seguinte comando:  

```powershell
Set-ExecutionPolicy RemoteSigned
```

- Para executar o script do instalador, execute o seguinte comando:  

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.ps1'))"
```

O script vai-nos fazer uma série de perguntas:

- Em qual pasta gostaria instalar? Podemos colocar uma rota alternativa, em este caso vou usar /home/usuario/oci-cli

```powershell
===> In what directory would you like to place the install? (leave blank to use 'C:\Users\usuario\lib\oracle-cli'): C:\Users\usuario\oracle-cli
-- Creating directory 'C:\Users\usuario\oracle-cli'.
-- We will install at 'C:\Users\usuario\oracle-cli'.
```

- Em qual pasta gostaria de instalar o executable oci?

```powershell
===> In what directory would you like to place the 'oci.exe' executable? (leave blank to use 'C:\Users\usuario\bin'): C:\Users\usuario\oci-cli\
```

- Em qual pasta gostaria de instalar os scripts da OCI?

```powershell
===> In what directory would you like to place the OCI scripts? (leave blank to use 'C:\Users\usuario\oci-cli\bin\scripts'): C:\Users\usuario\oracle-cli\bin\scripts
```

Vai-nos perguntar se quremos instalar pacotes opcionáis, apertamos ENTER pelo momento:  

```powershell
===> Currently supported optional packages are: ['db (will install cx_Oracle)']
What optional CLI packages would you like to be installed (comma separated names; press enter if you don't need any optional packages)?: PRESIONAMOS ENTER
```

Para terminar a configuración, vai-nos peguntar se desejamos modificar o PATH do nosso Powershell, colocamos Y depois ENTER:  

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

Fechamos PowerShell e vamos a abri-lô de novo.  

Verificamos se a instalação foi feita corretamente, vamos consultar a ajuda do programa:  

```powershell
oci --help
```

```plaintext
Usage: oci [OPTIONS] COMMAND [ARGS]...

  Oracle Cloud Infrastructure command line interface, with support for
  Audit, Block Volume, Compute, Database, IAM, Load Balancing, Networking,
  DNS, File Storage, Email Delivery and Object Storage Services.

  --- SAÍDA OMITIDA ---
```

## Configurando oci-cli

### Interactivo

Executamos o comando:  

```bash
oci setup config
```

Vai-nos fazer umas perguntas, primeiro a localização do arquivo config, vamos permitir que crie o arquivo na rota padrão:  

```bash
Enter a location for your config [/home/usuario/.oci/config]:
```

O seguinte que nos vai pedir, é o usuário OCID:  

```bash
Enter a user OCID:
```

Pelo que devemos iniciar sessão na nossa conta OCI, vamos ao icone de Perfil e click no nosso usuário cadastrado.

![Ecrã Principal](/images/oci-okit-linux-windows/oci-2.png "Ecrã Principal")

Copiamos a nossa OCID de usuário e a colocamos no parámetro user.  

![Ecrã Usuarios](/images/oci-okit-linux-windows/oci-3.png "Ecrã Usuarios")

Logo colocamos a OCID da conta (tenancy):  

```bash
Enter a tenancy OCID:
```

Opção Tenancy:  

![Ecrã Usuarios](/images/oci-okit-linux-windows/oci-4.png "Ecrã Usuarios")

Copiamos a nossa OCID da cuenta e a colocamos no parámetro tenancy:  

![Ecrã Usuarios](/images/oci-okit-linux-windows/oci-5.png "Ecrã Tenancy")

Depois nos vai pedir a região principal, no meu caso será us-ashburn-1:  

```bash
Enter a region (e.g. ap-chiyoda-1, ap-chuncheon-1, ap-hyderabad-1, ap-melbourne-
1, ap-mumbai-1, ap-osaka-1, ap-seoul-1, ap-sydney-1, ap-tokyo-1, ca-montreal-1,
ca-toronto-1, eu-amsterdam-1, eu-frankfurt-1, eu-zurich-1, me-jeddah-1, sa-saopa
ulo-1, uk-gov-cardiff-1, uk-gov-london-1, uk-london-1, us-ashburn-1, us-gov-ashb
urn-1, us-gov-chicago-1, us-gov-phoenix-1, us-langley-1, us-luke-1, us-phoenix-1
, us-sanjose-1): us-ashburn-1
```

Vai-nos perguntar se desejamos gerar uma nova chave o se queremos carregar uma existente, como já tihamos criado ao começo do artigo, presionamos "n":  

```bash
Do you want to generate a new API Signing RSA key pair? (If you decline you will
 be asked to supply the path to an existing key.) [Y/n]: n
```

E vamos indicar rota onde se encontra localizada a chave privada:  

```bash
Enter the location of your API Signing private key file: /home/vagrant/.oci/oci_api_key.pem
Fingerprint: 77:3d:77:73:::::::::::::::::
Config written to /home/usuario/.oci/config
```

### Manual

A maneira manual, é criar o arquivo de configuração em /home/usuario/.oci/config, com o seu editor de textos favorito:  

**Fedora 32:**  

```bash
vim /home/usuario/.oci/config
```

Em **Windows 10 Pro**, abrimos um Notepad e vamos escrever os seguintes parámetros:

```plaintext
[DEFAULT]
user=ocid1.user.oc1..aaaaaaaak6z......
fingerprint=3b:7e:37:ec:a0:86:1....
key_file=/home/usuario/oci-designer-toolkit/containers/oci/oci_api_key.pem
tenancy=ocid1.tenancy.oc1..aaaaaaaawpqblfem........
region=us-phoenix-1
```

Onde:  

- **user** é o dentificador OCID do usuario na OCI
- **fingerprint** é a impressão digital da chave ssh
- **key_file** é a tota onde se encontra armazenada a chave privada ssh (a guardamos em **/home/usuario/.oci/oci_api_key.pem**) No Windows substituimos pela rota do usuario (normalmente **C:\Users\usuario\.oci\oci_api_key.pem**)
- **tenancy** é o identificador OCID da tua conta
- **region** é a região padrão onde vais deployar a infraestrctura

Y pronto, deste maneira teremos instalado e configurado o cliente CLI da OCI em nosso sistema.  

Espero que tinham gostado deste tutorial, até a próxima!
