---
title: "Como Configurar Xming y Redireccionar Ventanas X11 en Microsoft Windows"
date: 2020-07-24
lastmod: 2020-07-24
author: "Enmanuel Moreira"
description: "En este articulo vamos a configurar el redireccionamiento de X11 a Microsoft Windows, usando Xming y SSH."
draft: false

resources:
- name: "featured-image"
  src: "featured-image.png"

categories: ["Linux"]
tags: ["linux","tutoriais"]

lightgallery: true
---

<!--more-->

Olá Pessoal, tudo bem?  

Há dias estive a falar com um SysAdmin, ele tinha uma necessidade muito específica: o software que estava a instalar em um servidor estava escrito em Java, e para configrar os parámetros do mesmo, precisava obrigatoriamente abrir a interface gráfica do configurador, porém, por tratar-se de um servidor em produção, o servidor não tinha o Servidor X11 instalado e além de isso, o servidor estava bloqueando o acesso para a Internet. A documentaçºao do software falava de maneira explicita que devia de ser configurado pela interface gráfica.  

Então, o qué fazer? Por sorte, levo muito tempo a utilizar duas ferramentas para fazer forwarding  (ou ver en termos mais simples) uma janela do Linux (X11) no Microsoft Windows. Estas duas ferramentas são: O Xming e o Putty. Neste tutorial vou-lhes explicar o processo.  

O **[Xming](https://sourceforge.net/projects/xming/files/Xming/)** é um software desenvolvido por Colin Harrison sendo o servidor líder para Microsoft Windows. Tem muitas funções, é simples, rápido, fácil de instalar (vamos ver logo) e, já que é Windows nativo independiente, é fácilmente portável (não precisa de uma instalação específica da máquina nem de acesso ao registo do Windows).  

Xming é totalmente seguro quando se usa com SSH e opcionalmente inclui um cliente Plink SSH melhorado e um pacote portável de substitução do PuTTY. Os instaladores do Xming incluem código executable e librarias. Foi open source até 2007 e a sua última versão foid a 6.9.0.31. Atualmente, é de código fechado e pode ser conseguido em **<http://www.straightrunning.com/XmingNotes/>** (já na sua versão 7.7.0.56) com uma donação de 10 libras esterlinas (aproximadamente uns USD 10.00) e podes receber atualizações por um ano.  

O **[Putty](https://www.putty.org/)** é um cliente SSH e Telnet, que foi desenvolvido originalmente pelo Simon Tatham para a plataforma Microsoft Windows. O PuTTY é um software open source cujo código fuente está disponível para qualquier que quisesse aportar a melhorar a ferramenta, além de ser ampliamente suportado pela comunidade de voluntários.  

Já com estas duas ferramentas, e uma PC ou uma Máquina Virtual com Linux (CentOS 8) e um PC Windows 10 (no meu caso), podemos começar!  

## Passo 1: Baixamos o software necessário na máquina cliente

***

Baixamos a versão atual do para Windows:  

* 32 bits: **<https://the.earth.li/~sgtatham/putty/latest/w32/putty-0.74-installer.msi>**
* 64 bits: **<https://the.earth.li/~sgtatham/putty/latest/w64/putty-64bit-0.74-installer.msi>**

Agora baixamos oXming desde a página do projecto em Sourceforge:  
**<https://sourceforge.net/projects/xming/files/Xming/6.9.0.31/Xming-6-9-0-31-setup.exe/download>**

Baixamos o pacote das fontes TrueType do Xming:
**<https://sourceforge.net/projects/xming/files/Xming-fonts/7.7.0.10/Xming-fonts-7-7-0-10-setup.exe/download>**

## Passo 2: Instalar o Putty

A instalação é bem simples (tens que ter privilégios de Administrador):  

Click em Next  

![Installer](/images/putty-xming/putty-0.png)

Podemos trocar a pasta destino, vou deixar a pasta destino padrão:  

![Destination Folder](/images/putty-xming/putty-1.png)

Deixamos padrão também:  

![Product Features](/images/putty-xming/putty-2.png)

Fim  

![Finish](/images/putty-xming/putty-3.png)

## Passo 2: Instalar o Xming

***

Iniciamos o instalador:  

![Installer](/images/putty-xming/xming-0.png)

Escolhemos a pasta, vou deixar a pasta destino padrão  

![Destination Folder](/images/putty-xming/xming-1.png)

Selecionamos os elementos a instalar, vou deixar em Full Installation  

![Menu Folder](/images/putty-xming/xming-2.png)

Click em Next  

![Select Components](/images/putty-xming/xming-3.png)

Criamos los acessos direitos para o Xming e o XLaunch (XLaunch é o configurador do Xming)  

![Additionals Tasks](/images/putty-xming/xming-4.png)

Começamos com a instalación  

![Ready to Install](/images/putty-xming/xming-5.png)

Iniciamos o Xming  

![Finish](/images/putty-xming/xming-6.png)

Na Barra de tarefas vai aparecer um Icone com uma X, onde vai ficar residente o Xming  

![Xming icon](/images/putty-xming/xming-7.png)

## Passo 3: Instalar as Xming-Fonts

***

Click em Next  

![Select Components](/images/putty-xming/xfonts-0.png)

Click em Install  

![Ready to Install](/images/putty-xming/xfonts-1.png)

Conluimos a instalação  

![Finish](/images/putty-xming/xfonts-2.png)

## Passo 4: Configuramos Xming con XLaunch

***

Abrimos o XLaunch  

Escolhemos a opção de multiples janelas:  

![Display Settings](/images/putty-xming/xlaunch-0.png)

Deixamos padrão, só para iniciar o cliente do Xming  

![Session Type](/images/putty-xming/xlaunch-1.png)

Se tiveremos alguns parámetros adicionais, os colocamos ali, senão Next  

![Additional Parameters](/images/putty-xming/xlaunch-2.png)

Se queremos salvar a configuração atual e poder brincar em um futuro com as opções, fazemos click em Save Configuration.  

![Finish](/images/putty-xming/xlaunch-3.png)

Agora devemos colocar o acesso direito do Xming na pasta C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp que é a pasta do inicio do Windows.  

## Paso 5: Modificar os valores do servidor SSH no Linux

***

Muito bem, nesta etapa, temso que ir ao nosso servidor Linux e modificar alguns parámetros do arquivo /etc/sshd/sshd_config com o nosso editor de textos favorito.  

```bash
sudo nano /etc/ssh/sshd_config
```

Tiramos o comentário (#) do seguinte valor.  

```plaintext
# X11Forwarding no
```

e trocamos a:

```plaintext
X11Forwarding yes
```

Salvamos as mudanças, e reiniciamos o servidor SSH.  

```bash
sudo systemctl restart sshd
```

## Psaso 6: Configurar o Putty

***

A primeira coisa a fazer, é ir ao Menu Connection -> SSH -> X11 e ativamos a opção Enable X11 forwarding:  

![Putty X11 forwarding](/images/putty-xming/putty-4.png)

Vamos agira ao menu menu Session, e colocamos o Endereço IP do nosso servidor, um nome do nosso perfil para o mesmo y fazemos click em Save  

![Putty Session](/images/putty-xming/putty-5.png)

Fazemos click em Load (para carregar a configuração) e depois Open  

Vai-nos mostrar esta mensagem, onde nos informa que a chave ssh del servidor não está aparelhada dentro dos hosts conhecidos (como este tutorial é uma prova de conceito não importa muito, porém, se vais implementar isto em um servidor em produção sempre tens que ter as tuas propias chaves), Fazemos click en Sim.  

![Putty keys](/images/putty-xming/putty-6.png)

Iniciamos sessão com o nosso usuário e palavra-chave:  

![Putty login](/images/putty-xming/putty-7.png)

Agora vamos a iniciar o programa glxgears como exemplo:

![Putty glxgears](/images/putty-xming/putty-8.png)

Agora abrimos Firefox:

![Putty Firefox](/images/putty-xming/putty-9.png)

Documentação: **<http://www.straightrunning.com/XmingNotes/manual.php>**

Espero que tinham gostado deste tutorial, até a próxima!
