---
title: "Como Configurar un Honeypot SSH y detectar Hackers en tus Servidores"
date: 2020-10-17
lastmod: 2020-10-17
author: Enmanuel Moreira
description: "Se tens um servidor e tu ou outra pessoa tinha iniciado sessão de maneira remota através de SSH, é provável que conheças a seguinte situação: Inicias sessão no teu servidor, revisas os logs de autenticação e descobres que alguem tinha tentado se conectar com as credenciáis padrão muitas vezes. Se tiveres uma palavra-chave forte e usas autenticaçao com chave pública, essas tentativas são bastante inofensivas, ainda asim é um pouco incómodo saber que alguem esteja constantemente tentando conseguir entrar ao servidor.  "
draft: false

resources:
- name: "featured-image"
  src: "featured-image.jpg"

categories: ["Linux"]
tags: ["linux", "centos", "rhel"]

lightgallery: true
---

<!--more-->

## Introdução

***

Se tens um servidor e tu ou outra pessoa tinha iniciado sessão de maneira remota através de SSH, é provável que conheças a seguinte situação: Inicias sessão no teu servidor, revisas os logs de autenticação e descobres que alguem tinha tentado se conectar com as credenciáis padrão muitas vezes. Se tiveres uma palavra-chave forte e usas autenticaçao com chave pública, essas tentativas são bastante inofensivas, ainda asim é um pouco incómodo saber que alguem esteja constantemente tentando conseguir entrar ao servidor.  

Neste tutorial vou-te ensinar uma forma bastante divertida de configurar o SSH para evitar este tipo de ataques e com algo de sorte podas fazer perder o tempo aos hackers.  

## Força Bruta

***

A maneira em que os hackers usualmente tentam acceder aos servidores é através de um método chamado "Força Bruta", eles têm uma lista das palavras-chaves mais populares (palavras-chaves não seguras) e vão tentar acceder mediante scripts automatizados pelo que, por padrão, o SSH não tem nenhum tipo de captcha ou mecanismo que expulsar a os atacantes despois de varias tentativas falhidas, é muito dificil de automatizar e não precisa de intervenção, o atacante puede simplesmente iniciar o seu script e continuar com a vida dele enquanto o script faz o trabalho sujo.  

Porém, nas especificaciones do SSH, se inclui uma coisa chamada "banner" o qual é básicamente um texto que se mostra a todos os que tentar iniciar sessão no servidor e não tem límite sobre quanto rempo pode ser e quando tempo pode tomar para mostra-lô.  

O [Chris Wellons](https://nullprogram.com/blog/2019/03/22/) tomou essa idea e desenvolveu um software chamado **endlessh** en cujo [GitHub](https://github.com/skeeto/endlessh) encontra-se o código.  

## ¿Cómo funciona endlessh?

***

Basicamente, tu configuras o servidor SSH real em uma porta diferente à real (por exemplo a 2222), depois eecutas endlessh na porta 22 e pretende ser o servidor SSH real. Cada vez que alguém tentar iniciar sessão, vai ser obrigado a ler uma messagem muito, mas muito longa infinitamente. Nunca haverá um timeout, não vai mostrar menssagem de error nenhuma, e vai mostrar caracteres estranhos até que o processo seja detido manualmente.  

Já que a maioria destes ataques são automatizados, pode levar muito tempo até que o atacante se dei conta que alguma coisa saiu errada. E sim, há muitoas maneiras normais e chatas de proteges os teus servidores como iptables ou fail2ban, mas com endlessh não vais estar a proteger os teus servidores dos atacantes, também vais fazer perder o valioso tempo deles em atacar outros servidores.  

## Instalando endlessh

***

Neste artigo voy utilizar o Ubuntu na versão 20.04.  

Instalamos dependencias:  

```bash
sudo apt-get install make gcc libc6-dev
```

Clonamos o repositório com o código fonte de endlessh:  

```bash 
git clone https://github.com/skeeto/endlessh
```

Entramos à pasta:  

```bash
cd endlessh
```

Compilamos:  

```bash
make
```

Colocamos o executable na pasta /usr/local/bin:  

```bash
sudo cp endlessh /usr/local/bin
```

Copiamos o arquivo de configuração do serviço endlessh:  

```bash
sudo cp util/endlessh.service /etc/systemd/system
```

Criamos a pasta onde vai a configuração padrão do serviço:  

```bash
sudo mkdir -p /etc/endlessh
```

Criamos o arquivo de configuração e escrevemos nele a porta 22:  

```bash
sudo echo "Port 22" | sudo tee /etc/endlessh/config
```

Iniciamos o serviço e o configuramos para que iniciar durante o inicio do sistema operacional:  

```bash
sudo systemctl enable --now endlessh
```

Verificamos que el serviço esteja a escutar:  

```bash
netstat -tupln | grep endlessh
```

Agora, abrimos um novo terminal e vamos tentar acceder ao nosso servidor, como podes ver, se eu especifico a porta 2222, que é nova porta na qual o servidor ssh real se está a executar, tudo bem, está a permitir iniciar sessão, mas se eu configurar na porta 22, não acontece coisa nenhuma.

Se colocarmos a opção:  

```bash
ssh usuario@host -p 22 -vvv
```

Veremos por detrás dos bastidores, que essas estranhas línhas aleatórias pelo banner, em realidade se acelera porque cad alinha demora uns 30 segundos em ser mostrada, e como eu diz anteriormente, o cliente realmente não trata esse banner como parte do processo de iniciar sessãp, assim que o atacante vai ficar atrapado nesse cenário. 

Espero que tinham gostado, até a próxima!
