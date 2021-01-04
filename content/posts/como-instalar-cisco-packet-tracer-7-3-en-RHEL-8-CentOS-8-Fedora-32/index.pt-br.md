---
title: "Como Instalar o Packet Tracer 7.3.x no RHEL 8 / CentOS 8 / Fedora 32 sem alien"
date: 2020-09-29
lastmod: 2020-09-29
author: Enmanuel Moreira
description: "Neste tutorial vamos instalar o Packet Tracer 7.3.1 em CentOS 8, porém é válido também para RHEL 8 e Fedora 32. É provável que seja necessário instalar algumas bibliotecas adiocionais e que possam variar de distro em distro"
draft: false

resources:
- name: "featured-image"
  src: "featured-image.png"

categories: ["Linux","Redes"]
tags: ["tutorial","redes"]

lightgallery: true
---

<!--more-->

Olá Pessoal, tudo bem?

Neste tutorial vamos instalar o Packet Tracer 7.3.1 em CentOS 8, porém é válido também para RHEL 8 e Fedora 32. É provável que seja necessário instalar algumas bibliotecas adiocionais e que possam variar de distro em distro.  

Agora, ¿por que isto sería necessário? Cisco sempre tinha suportado Ubuntu como distro para empacotar seu software, havia também um script para poder ser instalado. Infelizmente, Cisco anunciou que o Packet Tracer só ia estar disponível como pacote .deb a partir da versión 7.3 e que apenas sería suportado oficialmente no Ubuntu 18.04.03 LTS.  

É possível transformar o pacote .deb a .rpm com o programa **alien**, mas, com os passos que vou-lhes dizer, não vai ser necessário instalar alien no nosso sistema.  

## Baixando o Instalador .deb

***

Para descarregar o pacote .deb, vamos iniciar sessão iniciar sesión no sitio web de [Cisco Netacad](https://www.netacad.com), e ir ao menu Recursos -> Baixar Packet Tracer:  

![Ecrã0 Netacad](/images/instalar-packet-tracer-rpm/pt-0.png "Ecrã Netacad")

![Ecrã Downloads](/images/instalar-packet-tracer-rpm/pt-1.png "Ecrã Downloads")

## Desempacotar o Instalador

***

Agora vamos desempacotar o arquivo .deb:  

- Criamos uma pasta em /tmp onde vai armazenar os archivos desempacotados:  

```bash
mkdir /tmp/pt-instalador
```

- Copiamos o instalador .deb à pasta criada e entramos na mesma:  

```bash
cp PacketTracer_731_amd64.deb /tmp/pt-instalador
cd /tmp/pt-instalador
```

- Desempacotamos:  

```bash
ar -xv PacketTracer_731_amd64.deb
```

- Criamos las pastas control e data:  

```bash
mkdir /tmp/pt-instalador/{control,data}
```

- Desempacotamos os arquivos control e data (que estavam dentro do archivo .deb):  

```bash
tar -C control -xJf control.tar.xz
tar -C data -xJf data.tar.xz
```

- Entramos à pasta data:  

```bash
cd data
```

## Desinstalando a Versão Anterior de Packet Tracer

No caso de ter instalada uma versão anterior de Packet Tracer, vamos apagar a pasta onde se encontra instalado (vamos asumir que se encontra na pasta /opt/pt) e os arquivos da aplicação (icones, etc):  

```bash
sudo rm -rf /opt/pt
sudo rm -rf /usr/share/applications/cisco-pt7.desktop 
sudo rm -rf /usr/share/applications/cisco-ptsa7.desktop 
sudo rm -rf /usr/share/icons/hicolor/48x48/apps/pt7.png 
```

## Instalando e Configurando Packet Tracer

Asumindo que ainda estamos  na pasta /tmp/pt-instalador, vamos copiar as pastas:  

```bash
sudo cp -r usr / && \
sudo cp -r opt /
```

Agora vamos atualizar os icones y os arquivos para que o nosso sistema possa reconhece-lâs:  

```bash
sudo xdg-desktop-menu install /usr/share/applications/cisco-pt7.desktop 
sudo xdg-desktop-menu install /usr/share/applications/cisco-ptsa7.desktop 
sudo update-mime-database /usr/share/mime 
sudo gtk-update-icon-cache --force --ignore-theme-index /usr/share/icons/gnome 
sudo xdg-mime default cisco-ptsa7.desktop x-scheme-handler/pttp
```

Criamos un enlace simbólico do executável de Packet Tracer em /usr/local/bin:  

```bash
sudo ln -sf /opt/pt/packettracer /usr/local/bin/packettracer
```

Para acabar, vamos atualizar o arquivo /etc/profile para que aponte à pasta /opt/pt:  

```bash
sudo vim /etc/profile
```

Y colamos ao fim do arquivo o seguinte:  

```bash
PT7HOME=/opt/pt
export PT7HOME
QT_DEVICE_PIXEL_RATIO=auto
export QT_DEVICE_PIXEL_RATIO
```

Fechamos a nossa terminal, e abrimos uma nova para que possa tomar as mudanças. Comprovamos que esteja atualizado, fazendo uma chamada às variáveis de ambiente PT7HOME e QT_DEVICE_PIXEL_RATIO:  

```bash
echo $PT7HOME

```

Devería reponder:  

```bash
/opt/pt
```

```bash
echo $QT_DEVICE_PIXEL_RATIO
```

Também devería responder:  

```bash
auto
```

No terminal, executamos o comando packettracer ou no menú principal veremos o icone:

```bash
packettracer
```

![Ecrã APP](/images/instalar-packet-tracer-rpm/pt-2.png "Ecrã Principal Packet Tracer")

## Troubleshooting

No caso que Packet Tracer no iniciar, vamos copiar o script que faz a chamada ao executável packettracer para poder fazer debug:  

```bash
sudo cp /opt/pt/packettracer /opt/pt/ptdebug
```

Modificamos o arquivo /opt/pt/ptdebug, vamos modificar o redirecionamento /dev/null para poder visualizar as messagens da aplicação pela consola:  

```bash
sudo vim /opt/pt/ptdebug
```

Arquivo Original:

```bash
#!/bin/bash

echo Starting Packet Tracer 7.3.1

PTDIR=/opt/pt
export LD_LIBRARY_PATH=/opt/pt/bin
pushd /opt/pt/bin > /dev/null
./PacketTracer7 "$@" > /dev/null 2>&1
popd > /dev/null
```

Archivo Modificado:  

```bash
#!/bin/bash

echo Starting Packet Tracer 7.3.1

PTDIR=/opt/pt
export LD_LIBRARY_PATH=/opt/pt/bin
pushd /opt/pt/bin
./PacketTracer7 "$@"
popd
```

Guardamos as mudançãs, fechamos o editor e executamos ptdebug:  

```bash
sh /opt/pt/ptdebug
```

```bash
Starting Packet Tracer 7.3.1
/opt/pt/bin ~
Qt WebEngine ICU data not found at /opt/pt/bin/resources. Trying parent directory...
Installed Qt WebEngine locales directory not found at location /opt/pt/bin/translations/qtwebengine_locales. Trying application directory...
Qt WebEngine resources not found at /opt/pt/bin/resources. Trying parent directory...
```

Com ptdebug vamos poder ver se faltasse alguma que outra biblioteca por instalar e atuamos em consecuência.  

Isto é tudo por hoje, espero tinham gostado, até a próxima!
