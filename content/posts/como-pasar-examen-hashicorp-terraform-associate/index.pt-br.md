---
title: "Como passar o teste Oracle Cloud Infrastructure Operations Associate (1Z0-1067-19)"
date: 2020-08-18
lastmod: 2020-08-20
author: "Enmanuel Moreira"
description: "Em um post anterior falei do processo de estudar e fazer aquele teste. Hoje vou falar do teste Oracle Cloud Infrastructure Architect Associate (1Z0-1072-F)"
draft: true

resources:
- name: "featured-image"
  src: "featured-image.jpg"

categories: ["Certificações"]
tags: ["oracle", "oci", "cloud", "certificações"]

lightgallery: true
---

<!--more-->

Olá Pessoal, tudo bem?

Em um post anterior **[Como passar o teste Oracle Cloud Infrastructure Architect Associate (1Z0-1072-F)](https://itsimplenow.com/como-pasar-examen-oracle-cloud-infrastructure-architect-associate)** falei do processo de estudar e fazer esse teste. Hoje vou falar do teste **[Oracle Cloud Infrastructure Operations Associate (1Z0-1067-19)](https://www.oracle.com/cloud/iaas/training/operations.html)**

> Designed for professionals responsible for managing infrastructure solutions using Oracle Cloud Infrastructure services

> Este teste está estruturado para professionais responsáveis de administrar soluções de infraestrutura a utilizar os serviços da Oracle Cloud Infrastructure

{{< admonition note >}}
Em 15 de julho de 2020, a Oracle liberou uma nova versão do teste (o 1Z0-1067-20) o qual tem muito conteúdo novo.
{{< /admonition >}}

Os **[pontos](https://www.oracle.com/a/ocom/docs/oci-operations-associate-certification-2020-study-guide.pdf)** que devemos estudar são os seguintes:

1. Automating Cloud Tasks

* Use the OCI CLI to simplify repetitive tasks.
* Utilize configuration management tools to control infrastructure.
* Manage infrastructure as code.
* Explain user resource manager processes to implement infrastructure as code.

2. Performance Tuning and Troubleshooting

* Explain Troubleshooting resource availability and accessibility.
* Validate OCI performance.

3. Managing Cost

* Utilize billing tags to track cost.
* Explain how to Implement billing alerts.
* Leverage automation to control cost.

4. Security and Compliance

* Create secure access control policies.
* Leverage compartments for resource isolation.
* Audit cloud access.

5. Monitoring and Alerting

* Understand Metric Query Language (MQL)
* Create and managing alarms.
* Implement automated notifications.

6. Data Retention and Archival

* Use Object Storage Lifecycle policies for tiered data storage.
* Manage automated block storage and database backups.
* Implement cross-region data copy strategies.

7. Designing for cloud-scale agility

* Utilize edge services for automated failover / recovery.
* Implement hybrid network environments.

### Materiais de Estudo

***

Além do material gratuito que esteve disponível, usei os seguintes recursos:  

- **[Treino Oficial da Oracle Infrastructure Cloud Operations (Deves de ter uma conta na Oracle)](https://learn.oracle.com/ols/learning-path/managing-cloud-operations/35644/60972?xd_co_f=Mjc2NzJkMTExYzlhODQyNTcxZTE1NzkyNTAyNzgzMjQ=)**

- **[Teste de Prática](http://oukc.oracle.com/public/redir.html?type=player&offid=2481913383)**

- **[A documentação Oficial da OCI na Oracle](https://docs.cloud.oracle.com/pt-br/iaas/Content/home.htm)**  

- **[Guía de usuario da OCI](https://docs.cloud.oracle.com/en-us/iaas/pdf/ug/OCI_User_Guide.pdf)**  

- **[White Papers](https://docs.cloud.oracle.com/iaas/Content/General/Reference/aqswhitepapers.htm)**  

- **[Baixar Slides do Treino (ZIP)](https://download.oracle.com/ocomdocs/global/OCI-Operations-Associate-Cert-Content.zip)**  

### O Teste

***

- Custo atual: $150.00  
- Língua: Inglés (Infelizmente o teste não está disponível nem em Português nem no Espanhol)  
- Formato: Perguntas de Seleção Simples e Múltiple  
- Modalidade: Proctored  
- Duração: 85 minutos  
- Número de Perguntas: 55  
- Nota mínima para passar: 70%  
- Pode ser feito em um Centro Autorizado da Oracle no teu país ou em casa/escritório :)  

Após a experiência do teste Architect Associate, e depois de ver o conteúdo novo, gostei muito do uso do Terraform e do Ansible como ferramentas para fazer deploy e configurar uma infraestrutura na nuvém completa na OCI, o qual faz mais fácil a tarefa de administrar e vida mesma. A cli da OCI tem muitos parámetros que para aprende-lôs é necessário praticar muito, além de imaginar-se situaçoes hipotéticas para tentar fazer troubleshooting. E deves focar muito em budgets e boas práticas na OCI, isto é fundamental para aplica-lô em situações reais.  

Condiderando isto, dediquei aproximadamente 4 hrs por dia durante 9 días, praticar muito e com a base do conteúdo estudado para o teste OCI Architect Associate, passei o teste em 31/07/2020!!!

![exams-results3](/images/oci-cloud-operations/exam-score-report-1.png "Exam Score 31/07/2020")

Deves esperar 24 a 48 hrs para que chegue a teu correio eletrónico uma notificaçao com o certificado, o qual poderás baixa-lô na website de **[CertView de Oracle](https://certview.oracle.com/)**  

Adicionalmente, podes baixar tua Placa em **[YourAcclaim](https://www.youracclaim.com/)** e compartilhar a tua felicidade pelo Linkedin :)  

### Dicas no momento de fazer o teste

***

- Uma PC ou Laptop que tenha webcam e acesso ao microfone.  
- Uma boa conexão para Internet.  
- Tenta de iniciar sessão pelo menos 30 antes do teste e possas seguir os passos que te vai indicar o software da Pearson.  
- Ficar em um lugar tranquilo e bem iluminado, o proctor pode ser bem exigente ou indulgente.  
- Uma vez que o teste començar, não te podes levantar da cadeira, procura ir à casa-de-banho antes.  
- Ter o DNI em mão ou Passaporte **VIGENTE**  
- Navegadores suportados: Mozilla Firefox, Google Chrome, Microsoft Edge.  

Como comentario final, tem que dedicar muito tempo a estudar navegar pela Documentação (sobretudo a CLI da OCI). Praticar na Free Tier da OCI foi muito útil para poder passar um teste tão exigente.  

Espero que tinham gostado, até a próxima!
