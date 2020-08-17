---
title: "Como instalar Notion no Fedora 32"
date: 2020-08-01
lastmod: 2020-08-01
author: "Enmanuel Moreira"
description: "Neste post vamos instalar a ferramenta Notion no Fedora 32" 
draft: false

resources:
- name: "featured-image"
  src: "featured-image.jpg"

categories: ["Linux"]
tags: ["linux", "fedora","tutoriais"]

lightgallery: true
---

<!--more-->

Olá Pessoal, tudo bem?

Hoje queria-lhes falar de uma ferramenta que conhecí via **[4Fasters](https://4fasters.com.br)**: **[Notion](https://notion.so/)**  

O websiye da **[Notion](https://notion.so/)** define à ferramenta como:  
> Is a collaboration platform with markdown support that integrates kanban boards, tasks, wikis, and databases. The company claims to be an all-in-one workspace for note-taking, knowledge and data management, project and task management.  

Notion é um ótimo software para organizar a nossa vida, é tão potente que nos permite levar um registo das nossas tarefas, escrever notas, fazer gráficos, etc. O melhor é que as mudanças se sincronizam e armazenam na núvem pelo que se podem acessar em qualquer lugar e momento.  

Notion tem duas versões: Uma pessoal e outra de Pago. Os preços e os planos de assinatura se podem ver neste **[enlace](https://www.notion.so/pricing)**  

Suporta Microsoft Windows, MacOS, Android e iOS (além de ter uma versão Web) Infelizmente, ainda não tem uma versão oficial para o Linux e segundo o que podemos ler na conta da Notion no **[Twitter](https://twitter.com/NotionHQ)** não vai ser introduzido ao mercado Linux em um futuro próximo.  

{{< tweet 912737143327301634 >}}

Por sorte, através de Electon e com o apóio de um script, podemos compilar e instalar Notion em Fedora 32 a usar o instalador para Microsoft Windows.

### Passo 1: Instalamos Node.js e algumas dependências  

***

```bash
sudo dnf install nodejs p7zip-plugins ImageMagick fakeroot -y
```

### Passo 2: Instalamos dependências desde npm

***

```bash
sudo npm -g install asar electron-packager electron-installer-redhat electron-installer-debian
```

### Passo 3: Clonamos o repositório notion-rpm-builder  

***

```bash
git clone https://github.com/enmanuelmoreira/notion-rpm-builder
```

Entramos na pasta  

```bash
cd notion-rpm-builder
```

### Passo 4: Baixamos a última versão do Notion

***

Hoje a última versão é a 2.0.8  

```bash
wget 'https://desktop-release.notion-static.com/Notion%20Setup%202.0.8.exe' -O notion.exe
```

Mudamos os privilegios de execução ao script build.sh  

```bash
chmod +x build.sh
```

Executamos o script:  

```bash
./build.sh <platform>
```

Onde vai ser trocado **`<platform>`** pela versão do sistema operativo do instalador (neste caso Windows)  

```bash
./build.sh windows
```

Aguardamos uns poucos minutos, e o pacote já contruído devería estar na pasta dist/installers

### Passo 5: Instalamos o pacote contruído  

***

Instalamos o pacote notion-desktop

```bash
sudo rpm -i dist/installers/notion-desktop-2.0.8-1.x86_64.rpm
```

Y Pronto! Teremos o Notion listo instalado no nosso sistema.  

![Notion Desktop](/images/notion-fedora-32/notion-desktop.png "Ecrã Principal")  

Espero que tinham gostado, até a próxima!
