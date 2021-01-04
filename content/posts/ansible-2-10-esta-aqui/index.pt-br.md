---
title: "Ansible 2.10 está aquí"
date: 2020-09-24
lastmod: 2020-09-24
author: Enmanuel Moreira
description: "Em 22/09/2020, foi anunciada a liberação da versão 2.10 de Ansible, a cual traz muitas novedades e neste artigo vou te contar!"
draft: false

resources:
- name: "featured-image"
  src: "featured-image.png"

categories: ["Automatização"]
tags: ["infraestrutura", "devops","automatização", "cloud"]

lightgallery: true
---

<!--more-->

Olá Pessoal, tudo bem?

Em 22/09/2020, foi [anunciado](https://groups.google.com/g/ansible-project/c/yFnMgbjqYnU) a liberação da versão 2.10 de Ansible, a cual traz muitas novedades e neste artigo vou te contar!

## Actualizando Ansible de 2.9.x a 2.10

***

Neste momento há um pequeno bug quando tentamos atualizar Ansible desde a versão 2.9.x, pelo que se recomenda desintalar primeiro Ansible 2.9.x:  

```bash
$ pip uninstall ansible
```

E depois instalar Ansible 2.10:  

```bash
$ pip install ansible==2.10 --user
```

## Mudanças Importantes

***

Segundo o [anuncio](https://people.redhat.com/mlessard/ansiblemtl/presentations/avril2020/WhatsnewwithAnsible2.10.pdf) pela Red Hat, Ansible vendrá com alguns dos seguintes mudanças:  

- Todos os playbooks que tinhas sido escritos em Ansible 2.9.x y sejan executados na nova versão versión 2.10, vão manter a compatilidade. No caso que alguma playbook não funcione correctamente, esta deverá ser revisada para verificar que não seja um bug da playbook.  

- Uns das mudanças mais importantes é que agora algumas coleções vão formar parte de Ansible 2.10: <https://github.com/ansible-community/ansible-build-data/blob/main/2.10/CHANGELOG-v2.10.rst>  

A Lista é bastante longa, assim que te recomendo ve-lâ: <https://github.com/ansible-community/ansible-build-data/blob/2.10.0/2.10/ansible-2.10.0.deps>  

E cá -> <https://github.com/ansible-collections>  

- Uns dos problemas principais que Ansible tinha, é a grande diferença de versões entre a versão de código aberto (gerenciada pela comunidade e com aportes voluntarios) e as que vem dos clientes o desenvolvedores da Red Hat, os quais tinham pelo menos 4500 objetos entre módulos, roles, etc; onde podiam existir dois módulos para a mesma tarefa(por exemplo: Helm) Por esta mesma razão, se decidiu separar os módulos e plugins "base" y colocar os demais como Ansible Collections (roles, módulos, plugins, documentação, etc), para assim  organiza-lôs e garatir um ersionado correito e semantico, além que a chamada de estas coleções seja rápida e facil de portar. Pelo que, que ao momento da instalação de Ansible 2.10, vai ser instalado também o pacote chamado  **ansible-core** que os vai conter.  

- No ldo do Microsoft Windows, já não vão ser suportados os sistemas operaciones: Windows Server 2008, 2008 R2 e Windows 7. Para mais informação: <https://docs.ansible.com/ansible/latest/user_guide/windows_faq.html#windows-faq-server2008>

- Está disponível a guía para portar os nossos playbooks feitos na versão 2.9.x para a 2.10: <https://docs.ansible.com/ansible/latest/porting_guides/porting_guide_2.10.html>

![Ansible 2.10](/images/ansible-2-10/ansible-2.10.png "Mudanças em Ansible 2.10")  

## Chamando uma Coleção em Ansible

Por exemplo, na versão 2.9.x, para poder criar um banco de dados MySQL, podíamos usar o seguinte código:  

```yml
name: Create a new database with name 'bobdata'
  mysql_db:
    name: bobdata
    state: present
```

Na versão 2.10, devemos incluir a coleção das tarefas que podem ser feitas com MySQL:  

```yml
- name: Create a new database with name 'bobdata'
  community.mysql.mysql_db:
    name: bobdata
    state: present
```

Onde chamamo à coleção community.mysql e ao modulo mysql_db que permite criar o banco de dados no servidor.  

A lista organizada e completa das coleções, poderás ve-lâs cá: <https://docs.ansible.com/ansible/latest/collections/>  

## Ansible Fest 2020

***

Para terminar, lembro-lhes que se vem o Ansible Fest em 13 e 14 de Outubro de 2020, a cual será virtual e poderam se cadastrar cá: <https://www.ansible.com/ansiblefest>

![Ansible Fest](/images/ansible-2-10/ansible-fest2020.png "Ansible Fest 2020")  

Espero que tinham gostado, até a próxima!
