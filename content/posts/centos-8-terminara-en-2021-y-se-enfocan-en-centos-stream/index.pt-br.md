---
title: "Suporte de CentOS vai acabar em 2021 e a equipa vai-se enfocar em CentOS Stream"
date: 2020-10-08
lastmod: 2020-10-08
author: Enmanuel Moreira
description: "Se alguma vez precisaste de Linux para tua empresa com um soporte oficial, Red Hat vende-te o suporte para seus sistemas operacionais: Red Hat Enterprise Linux. A maioria dos mortais usamos uma alternatica que se contrui da base do mesmo codigo fonte da RHEL, onde são tiradas todas as marcas registradas e qualquier coisa que tenha relação com isso: CentOS"
draft: true

resources:
- name: "featured-image"
  src: "featured-image.jpg"

categories: ["Linux"]
tags: ["linux", "centos", "rhel"]

lightgallery: true
---

<!--more-->

Olá Pessoal, tudo bem?

Se alguma vez precisaste de Linux para tua empresa com um soporte oficial, Red Hat vende-te o suporte para seus sistemas operacionais: Red Hat Enterprise Linux. A maioria dos mortais usamos uma alternatica que se contrui da base do mesmo codigo fonte da RHEL, onde são tiradas todas as marcas registradas e qualquier coisa que tenha relação com isso: [CentOS](https://www.centos.org/).  

Há alguns anos, CentOS virou-se na distro baseada em Red Hat mais popular. Em um [sorpresivo movimento](https://blog.centos.org/2020/12/future-is-centos-stream/), CentOS está mudando as coisas oferecendo uma versão rolling release: CentOS Stream.  

Uma distribuição distro rolling release é aquila que inclui todos os componentes mais atuáis do sistema operacional. Isto leva a que o software não possa ser considerado "maduro" (software com características muito novas, software não provado, bugs, etc) para entrar em um ambiente de produção. Exemplos dessas distros podem ser: Arch Linux, Manjaro, OpenSUSE Tumbleweed, Fedora, etc.  

***

## Anúncio Oficial

***

Segundo o anúncio [oficial](https://lists.centos.org/pipermail/centos-announce/2020-December/048208.html):  

> The future of the CentOS Project is CentOS Stream, and over the next year we’ll be shifting focus from CentOS Linux, the rebuild of Red Hat Enterprise Linux (RHEL), to CentOS Stream, which tracks just ahead of a current RHEL release. CentOS Linux 8, as a rebuild of RHEL 8, will end at the end of 2021. CentOS Stream continues after that date, serving as the upstream (development) branch of Red Hat Enterprise Linux. When CentOS Linux 8 (the rebuild of RHEL8) ends, your best option will be to migrate to CentOS Stream 8, which is a small delta from CentOS Linux 8, and has regular updates like traditional CentOS Linux releases. If you are using CentOS Linux 8 in a production environment, and are concerned that CentOS Stream will not meet your needs, we encourage you to contact Red Hat about options.

Em português:  

> O futuro do Projeto CentOS é o CentOS Stream e, no próximo ano, mudaremos o foco do CentOS Linux, reconstruindo o Red Hat Enterprise Linux (RHEL), para o CentOS Stream, que funciona um pouco antes da versão atual do RHEL. O CentOS Linux 8, como uma reconstrução do RHEL 8, terminará no final de 2021. O CentOS Stream continua após essa data e atua como o branch upstream (desenvolvimento) do Red Hat Enterprise Linux. Quando o CentOS Linux 8 terminar (reconstruindo o RHEL8), sua melhor aposta será migrar para o CentOS Stream 8, que é um pequeno delta do CentOS Linux 8 e tem atualizações regulares como as versões tradicionais do CentOS Linux. Se você estiver usando o CentOS Linux 8 em um ambiente de produção e estiver preocupado que o CentOS Stream não atenda às suas necessidades, recomendamos que você entre em contato com a Red Hat para obter as opções.

***

## ¿Como serán gestionados os CVE no CentOS Stream?

***

> Security issues will be updated in CentOS Stream after they are solved in the current RHEL release. Obviously, embargoed security releases can not be publicly released until after the embargo is lifted. While there will not be any SLA for timing, Red Hat Engineers will be building and testing other packages against these releases. If they do not roll in the updates, the other software they build could be impacted and therefore need to be redone. There is therefore a vested interest for them to get these updates in so as not to impact their other builds and there should be no issues getting security updates.

Se não percebiste nenhuma coisa até agora:  

> Os problemas de segurança serão atualizados no CentOS Stream após serem resolvidos na versão atual do RHEL. Obviamente, as liberações de segurança embargadas não podem ser publicadas até que o embargo seja levantado. Embora não haja SLAs por enquanto, os engenheiros da Red Hat irão construir e testar outros pacotes com essas versões. Se eles não distribuírem as atualizações, o outro software que eles construíram pode ser afetado e, portanto, precisa ser refeito. Portanto, há um grande interesse para eles em obter essas atualizações para não afetar suas outras compilações e não deve haver problemas para obter atualizações de segurança.

É dizer, os usuários de CentOS Stream poderão provar novas caracteristicas de RHEL o que vai permiritr reportar falhas, porém, isto não quer dizer que poderão obter atualizaçoes de segurança por parte da Red Hat. Estranho tudo isto.  

***

## RHEL Beta?

***

Segundo o [FAQ](https://centos.org/distro-faq/)

> No. CentOS Stream will be getting fixes and features ahead of RHEL. Generally speaking, we expect CentOS Stream to have fewer bugs and more runtime features than RHEL until those packages make it into the RHEL release.

Chama a atenção que estas mudanças não vão afectar ao CentOS 7 e que vai seguir recebendo atualizações de segurança até 2024.  

**¿Pode a comunidade continuar desenvolvendo / recontruindo o CentOS Linux?**
A própia Red Hat disse que não colocaram nem hardware, nem recursos nem pediram volutários para trabalhar nesse projeto, nem permitiram que a marca seja usada para tal fim, já que sintem que adultera o que estão a tratar de fazer com o novo CentOS Stream. Dito isto, o código é de código aberto e não intentaram evitar que ningém escolha usa-lô ou criar os seus próprios pacostes a partir do código.  

***

## Conclusões

***

A verdade que é uma jogada muito estranha por parte da Red Hat. Desde que foi comprada pela IBM em 2018 tinha estado involucrada em decissões questionáveis e esta não é a exepção. Muitas pequenas empresas que usam CentOS porque é essecialmente um RHEL além de que resulta bastante custoso o suporte, migraram de CentOS 7 a 8 para poder ter o seu centro de dados ao día (alías que o CentOS 8 teria suporte até 2029) com todo o tempo e recursos gastados.  

CentOS foi a nossa “plataforma de provas sensata” para MySQL, PHP, Nginx, Java e muitos outros aplicativos. Uma vez que o aplicativo estiver pronto, podiamos fazer a implementação em um clúster de  RHEL 8. Claro que podemos obter uma subscripção de desenvolvedor da RHEL, mas apenas se podem anadir uma susbcripção de desenvolvedor da Red Hat sem custo a uma conta de usuário para tal fim.  Então, se uma empresa pequena tem sete desenvolvedores, seis desenvoledores adicionáis podem criar suas próprias contas de usuário pela developers.redhat.com, tivendo que lidar com essas contas adicionáis. Porém, se os desenvolvedores quiserem ter um clon da gratuíto, a seguinte melhor opçao poderia ser a [Oracle Linux](https://www.oracle.com//linux/)  

En efecto, muchos usuarios no están contentos (incluido yo) Muy probablemente veremos una bifurcación del proyecto, lo que va a fragmentar más los esfuerzos para hacer que Linux tenga mucho mas penetración de mercado (no olvidemos lo que le pasó a [Scientific Linux](https://lwn.net/Articles/786422/)) y que esto puede significar que los proyectos mueran mucho más rápido por falta de apoyo de la comunidad.  

Só o tempo  el tiempo lo dirá...  

![Here we go](/images/centos8-ha-muerto/nixcraft.jpg "IBM: Here we go again!")  

Espero que tinham gostado, até a próxima!
