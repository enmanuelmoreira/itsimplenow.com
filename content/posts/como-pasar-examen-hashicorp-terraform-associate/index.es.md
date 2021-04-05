---
title: "Como pasar el examen Hashicorp Terraform Associate"
date: 2021-04-04
lastmod: 2021-04-05
author: "Enmanuel Moreira"
description: "Hoy les contaré mi experiencia para estudiar y pasar el examen Hashicorp Terraform Associate."
draft: false

resources:
- name: "featured-image"
  src: "featured-image.jpeg"

categories: ["Certificaciones"]
tags: ["terarform", "iac", "cloud", "certificaciones", "automatizacion", "infraestructura"]

lightgallery: true
---

<!--more-->

¡Hola a todos!

Hoy les contaré mi experiencia para estudiar y pasar el examen Hashicorp Terraform Associate.  

[Hashicorp](https://www.hashicorp.com/) es una de las empresas lideres en soluciones en infraestructura. Posee herramientas muy conocidas en el mercado, tales como Vagrant (aprovisionador de infraestructura física), [Terraform](https://www.terraform.io/) que es una herramienta de infraestructura como código, nos permite a través de código, definir y desplegar recursos tanto en la nube como en servidores físicos.  

Los **[tópicos](https://learn.hashicorp.com/tutorials/terraform/associate-study?in=terraform/certification)** que debemos estudiar son los siguientes:  

* 1	Understand infrastructure as code (IaC) concepts  
  * 1a	Explain what IaC is  
  * 1b	Describe advantages of IaC patterns  
* 2	Understand Terraform's purpose (vs other IaC)  
  * 2a	Explain multi-cloud and provider-agnostic benefits  
  * 2b	Explain the benefits of state  
* 3	Understand Terraform basics
  * 3a	Handle Terraform and provider installation and versioning
  * 3b	Describe plugin based architecture
  * 3c	Demonstrate using multiple providers
  * 3d	Describe how Terraform finds and fetches providers
  * 3e	Explain when to use and not use provisioners and when to use local-exec or remote-exec
* 4	Use the Terraform CLI (outside of core workflow)
  * 4a	Given a scenario: choose when to use terraform fmt to format code
  * 4b	Given a scenario: choose when to use terraform taint to taint Terraform resources
  * 4c	Given a scenario: choose when to use terraform import to import existing infrastructure into your Terraform state
  * 4d	Given a scenario: choose when to use terraform workspace to create workspaces
  * 4e	Given a scenario: choose when to use terraform state to view Terraform state
  * 4f	Given a scenario: choose when to enable verbose logging and what the outcome/value is
* 5	Interact with Terraform modules
  * 5a	Contrast module source options
  * 5b	Interact with module inputs and outputs
  * 5c	Describe variable scope within modules/child modules
  * 5d	Discover modules from the public Terraform Module Registry
  * 5e	Defining module version
* 6	Navigate Terraform workflow
  * 6a	Describe Terraform workflow ( Write -> Plan -> Create )
  * 6b	Initialize a Terraform working directory (terraform init)
  * 6c	Validate a Terraform configuration (terraform validate)
  * 6d	Generate and review an execution plan for Terraform (terraform plan)
  * 6e	Execute changes to infrastructure with Terraform (terraform apply)
  * 6f	Destroy Terraform managed infrastructure (terraform destroy)
* 7	Implement and maintain state
  * 7a	Describe default local backend
  * 7b	Outline state locking
  * 7c	Handle backend authentication methods
  * 7d	Describe remote state storage mechanisms and supported standard backends
  * 7e	Describe effect of Terraform refresh on state
  * 7f	Describe backend block in configuration and best practices for partial configurations
  * 7g	Understand secret management in state files
* 8	Read, generate, and modify configuration
  * 8a	Demonstrate use of variables and outputs
  * 8b	Describe secure secret injection best practice
  * 8c	Understand the use of collection and structural types
  * 8d	Create and differentiate resource and data configuration
  * 8e	Use resource addressing and resource parameters to connect resources together
  * 8f	Use Terraform built-in functions to write configuration
  * 8g	Configure resource using a dynamic block
  * 8h	Describe built-in dependency management (order of execution based)
* 9	Understand Terraform Cloud and Enterprise capabilities
  * 9a	Describe the benefits of Sentinel, registry, and workspaces
  * 9b	Differentiate OSS and TFE workspaces
  * 9c	Summarize features of Terraform Cloud  

### Materiales de Estudio

***

Además de la **[documentación oficial](https://www.terraform.io/docs/index.html)**, utilicé los siguientes recursos:  

- Curso de Udemy **[Hashicorp Certified: Terraform Associate 2021](https://www.udemy.com/course/hashicorp-certified-terraform-associate/)** de [Jacob Jones](https://www.udemy.com/user/jacob-jones-3/), estuvo varios meses grátis debido a que el curso tenía dos horas o menos de duración, ahora cuesta USD 29.99, aunque tiene otro que es grátis: **[Terraform 101](https://www.udemy.com/course/terraform-101/)**  

- El libro **[HashiCorp Terraform Certified Associate Preparation Guide](https://leanpub.com/terraform-certified)** de [Ned Bellavance](https://leanpub.com/u/edwardbellavanceuvE79nd2fzVVXRcxlTG1pg) y [Adin Ermie](https://leanpub.com/u/AErmie)  

- El libro **[Terraform Cookbook](https://www.amazon.com/Terraform-Cookbook-Efficiently-Infrastructure-platforms-ebook/dp/B08H8XD9W4)** de Mitchell Hashimoto  

- El **[Curso de Terraform](https://www.youtube.com/watch?v=ec4qHgJQM7c&list=PLfW3im2fiA7XDjPgS9uzgv5Zeyhi_QE9Y)** de Miguel Arranz en su **[canal de Youtube](https://www.youtube.com/c/MiguelArranz)**. Tiene otro **[canal](https://www.youtube.com/channel/UCjeMXFMvba_j7vE6vZKus-Q)** el cual recomiendo ampliamente suscribirse)  

### El Examen

***

- El registro se hace a través de PSI con tu cuenta GitHub, haciendo click [aquí](https://hashicorp-certifications.zendesk.com/hc/en-us/articles/360049382552)
- Costo actual: $70.50 (Pueden aplicar impuestos adicionales dependiendo de tu país)  
- Idioma: Inglés, Español (Yo la tomé en Inglés)  
- Formato: Preguntas de Selección Simple y Múltiple  
- Modalidad: Proctored  
- Duración: 60 minutos  
- Número de Preguntas: 57  
- Nota mínima para aprobar: 60%  
- Puedes presentarlo desde la comodidad de tu casa.  

A pesar de ser una prueba de selección simple y múltiple, recomiendo enormente practicar y mucho, estar claros con los conceptos como inmutabilidad, idempotencia, recursos, el ciclo de trabajo de Terraform y estudiar mucho el servicio de Terraform Cloud.

Recomiendo también saber al menos, de AWS, crear una cuenta y jugar con el stack gratuito, así podrás entender mejor los conceptos, crear recursos, destruirlos, hacerles taint a un recurso especifico, importar recursos ya existentes desde AWS, etc.  

Con esto en consideración, dedicándole aproximadamente 2 hrs por dia durante 15 días, practicar mucho, presenté el 04/04/2021 y pasé el examen!!!  

![exams-results](/images/terraform-associate/resultados.png "Exam Score 04/04/2021")

Lo que más me sorprendió fue que el resultado del examen me lo dieron casi de inmediato. Me llegó un email con los mismos y pude verlo tambien en la página PSI.  

Adicionalmente, puedes descargar tu Placa en **[YourAcclaim](https://www.youracclaim.com/)** y compartir tu felicidad por Linkedin :)  

La única queja que tengo (y vi un par más en LinkedIn), es que el soporte técnico de PSI tarda mucho, me conecté para hacer el pre-examen a las 9:35 hora local, el examen comenzó justo a las 10:00 horas, al cabo de 20 minutos el programa se cerró de manera forzada, me volví a conectar y pasó al menos 45 minutos para que alguien de soporte me atendiera (todo en inglés, btw), y sin mencionar que tuve que hacer el registro de mi ID, tomar video a mi entorno de trabajo, sacarme la selfie... Pues, PSI tiene muchas cosas por mejorar si quiere hacerle la competencia a Pearson. Terminé el examen a las 11:50 hora local.  

### Consejos al momento de presentar la prueba

***

- Debes tener una PC o Laptop que tenga cámara web y acceso al micrófono.  
- Una buena conexión a Internet.  
- Trata de iniciar sesión al menos 30 minutos antes de la prueba, para que puedas seguir los pasos que te indica el software de Pearson.  
- Estar en un lugar tranquilo y bien iluminado, depende del proctor que te toque puede ser bastante laxo o bastante exigente.  
- Una vez comience el examen, no te vas a poder levantar del asiento, por lo que trata de ir al baño antes.  
- Tener tu DNI a la mano o Pasaporte **VIGENTE**  
- Navegadores soportados: Mozilla Firefox, Google Chrome, Microsoft Edge.  

Como comentario final, aparte del tiempo de estudio, práctica y entender los conceptos, el día del examen debes tener mucha pero mucha paciencia, si bien es un examen nivel entry, debes entender que digerir conceptos de IaC toma tiempo, leer bien las preguntas e ir marcando aquellas en las que tengas dudas. También armarte de mucha paciencia el día del examen por lo que comenté recien sobre el soporte por parte de PSI.  

Espero les haya gustado ¡Hasta la próxima!

## Apoya este Proyecto!!!

Si te pareció útil este artículo y el proyecto en general, considera brindarme un café :)

<style>.bmc-button img{height: 34px !important;width: 35px !important;margin-bottom: 1px !important;box-shadow: none !important;border: none !important;vertical-align: middle !important;}.bmc-button{padding: 7px 15px 7px 10px !important;line-height: 35px !important;height:51px !important;text-decoration: none !important;display:inline-flex !important;color:#ffffff !important;background-color:#5F7FFF !important;border-radius: 8px !important;border: 1px solid transparent !important;font-size: 24px !important;letter-spacing: 0.6px !important;box-shadow: 0px 1px 2px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;margin: 0 auto !important;font-family:'Cookie', cursive !important;-webkit-box-sizing: border-box !important;box-sizing: border-box !important;}.bmc-button:hover, .bmc-button:active, .bmc-button:focus {-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;text-decoration: none !important;box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;opacity: 0.85 !important;color:#ffffff !important;}</style><link href="https://fonts.googleapis.com/css?family=Cookie" rel="stylesheet"><a class="bmc-button" target="_blank" href="https://www.buymeacoffee.com/enmanuelmoreira"><img src="https://cdn.buymeacoffee.com/buttons/bmc-new-btn-logo.svg" alt="Buy me a coffee"><span style="margin-left:5px;font-size:24px !important;">Buy me a coffee</span></a>
