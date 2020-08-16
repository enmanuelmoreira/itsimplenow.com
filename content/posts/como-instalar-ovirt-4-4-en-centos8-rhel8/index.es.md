---
title: "Como instalar oVirt 4.4 en Centos 8 - RHEL 8"
date: 2020-08-04
lastmod: 2020-08-04
author: "Enmanuel Moreira"
description: "En este articulo vamos a configurar un servidor Self-hosted (standalone) de oVirt en CentOS - RHEL 8"
draft: false

resources:
- name: "featured-image"
  src: "featured-image.png"

categories: ["Virtualización"]
tags: ["ovirt", "virtualización", "centos", "rhel"]

lightgallery: true
---

<!--more-->

Hola chiquillos!  

El día de hoy, vamos a instalar un servidor oVirt en Centos 8 / RHEL 8.  

### ¿Qué es Ovirt?

oVirt es una solución de virtualización libre y de código abierto, usado en sistemas Linux tales como Fedora, CentOS y RHEL. Viene a ser la alternativa libre de VMware Sphere en Linux. La comunidad oVirt es soportada por Red Hat y es considerado como un proyecto de suma importancia dentro del catalogo de productos de Red Hat, llamado Red Hat Enterprise Virtualization (RHEV)  

La pagina del proyecto oVirt es: **www.ovirt.org**  

oVirt consiste de dos componentes principales:  

* oVirt Engine  
* oVirt Node  

oVirt Engine es la interfaz gráfica web en la que se pueden gestionar máquinas virtuales, computo, redes y recursos de almacenamiento.  

oVirt Node es un servidor RHEL / CentOS o Fedora, en el que el servicio vdsm actúa como actua como Hypervisor (KVM) en la cual todas las maquinas virtuales son creadas.  

En este post vamos a instalar la ultima versión de oVirt 4.4.0 en CentOS 8.2 / RHEL 8.2 de manera standalone, es decir, vamos a instalar el Engine y el Node en un solo servidor con almacenamiento local como prueba de concepto. En otro post explicaré como hacerlo en dos máquinas o más distintas, con sus diferentes opciones de almacenamiento (NFS, GlusterFS, etc)  

Para efectos de este tutorial, vamos a trabajar con las siguientes configuraciones:  

Nombre FQDN: ovirt.example.com  

IP: 192.168.1.30  

Actualizamos las entradas en el archivo /etc/hosts (con su editor de textos de confianza) por si no tenemos resolución de nombres por DNS, para que quede de esta forma:  

```plaintext
192.168.1.30 engine.example.com engine
```

Además, deberemos tener una entrada en el servidor DNS o utilizar dnsmasq para que pueda resolver la dirección IP.  

### Requerimientos de Hardware  

***

| Hardware        |      Minimo      |  Recomendado  |
|:---------------:|:----------------:|:-------------:|
| CPU (con soporte para Intel® 64 o AMD64 CPU, AMD-V™ o Intel VT®) |  CPU Dual Core. | CPU Quad Core o con mas núcleos. |
| Memoria |    4 GB de RAM, sin embargo, la misma no debe estar siendo consumida por procesos adicionales. |  16 GB de RAM. |
| Almacenamiento | 25 GB de espacio disponible. |    50 GB de espacio disponible. |
| Red           |  1 NIC de al menos un ancho de banda de 1 Gbps.  |  1 NIC de al menos un ancho de banda de 1 Gbps. |

Con el siguiente comando verificamos si nuestra CPU soporta virtualizacion:  

```bash
[root@ovirt ~]# grep -E 'svm|vmx' /proc/cpuinfo | grep nx
```

### Paso 1: Actualizamos el Servidor  

***

```bash
[root@ovirt ~]# dnf update -y
```

Una vez instaladas las actualizaciones, reiniciamos el servidor (en caso de haber actualizaciones de Kernel):  

```bash
[root@ovirt ~]# systemctl reboot
```

### Paso 2: Habilitamos los repositorios de oVirt  

***

Los paquetes de software necesarios para desplegar la plataforma oVirt, no se encuentran disponibles por defecto en los repositorios de CentOS /RHEL, por lo que los añadimos el repositorio de oVirt:  

```bash
[root@ovirt ~]# dnf install http://resources.ovirt.org/pub/yum-repo/ovirt-release44.rpm -y
```

### Paso 3: Habilitamos los módulos javapackages-tools, pki-deps y PostgreSQL 12  

***

```bash
[root@ovirt ~]# dnf module enable javapackages-tools pki-deps postgresql:12 -y
```

### Paso 4: Instalamos la plataforma oVirt Engine  

***

```bash
[root@ovirt ~]# dnf install ovirt-engine -y
```

### Paso 5: Iniciamos el instalador de oVirt Engine  

***

Ejecutamos en la terminal el comando:

```bash
[root@ovirt ~]# engine-setup --generate-answer=/root/answer.txt
```

Con esto iniciará el instalador de oVirt Engine, en la que hará una serie de preguntas durante la instalacion y guardará las respuestas a un archivo. Luego por si queremos reutilizarlo para automatizar la instalación.  

Respondemos las preguntas del instalador, para escoger la opción por defecto pulsamos ENTER, sino, escogemos entre las opciones que nos muestra:  

```console

[ INFO  ] Stage: Initializing
[ INFO  ] Stage: Environment setup
          Configuration files: /etc/ovirt-engine-setup.conf.d/10-packaging-jboss.conf, /etc/ovirt-engine-setup.conf.d/10-packaging.conf
          Log file: /var/log/ovirt-engine/setup/ovirt-engine-setup-20200715145028-j2fmin.log
          Version: otopi-1.9.2 (otopi-1.9.2-1.el8)
[ INFO  ] Stage: Environment packages setup
[ INFO  ] Stage: Programs detection
[ INFO  ] Stage: Environment setup (late)
[ INFO  ] Stage: Environment customization

          --== PRODUCT OPTIONS ==--

          Configure Cinderlib integration (Currently in tech preview) (Yes, No) [No]: No
          Configure Engine on this host (Yes, No) [Yes]: Yes

          Configuring ovirt-provider-ovn also sets the Default cluster's default network provider to ovirt-provider-ovn.
          Non-Default clusters may be configured with an OVN after installation.
          Configure ovirt-provider-ovn (Yes, No) [Yes]: Yes
          Configure WebSocket Proxy on this host (Yes, No) [Yes]: Yes

          * Please note * : Data Warehouse is required for the engine.
          If you choose to not configure it on this host, you have to configure
          it on a remote host, and then configure the engine on this host so
          that it can access the database of the remote Data Warehouse host.
          Configure Data Warehouse on this host (Yes, No) [Yes]: Yes
          Configure Grafana on this host (Yes, No) [Yes]: Yes
          Configure VM Console Proxy on this host (Yes, No) [Yes]: Yes

          --== PACKAGES ==--

[ INFO  ] Checking for product updates...
[ INFO  ] No product updates found

          --== NETWORK CONFIGURATION ==--

          Host fully qualified DNS name of this server [ovirt.example.com]: ENTER
[WARNING] Failed to resolve ovirt.example.com using DNS, it can be resolved only locally

          Setup can automatically configure the firewall on this system.
          Note: automatic configuration of the firewall may overwrite current settings.
          Do you want Setup to configure the firewall? (Yes, No) [Yes]: Yes
[ INFO  ] firewalld will be configured as firewall manager.

          --== DATABASE CONFIGURATION ==--

          Where is the DWH database located? (Local, Remote) [Local]: ENTER

          Setup can configure the local postgresql server automatically for the DWH to run. This may conflict with existing applications.
          Would you like Setup to automatically configure postgresql and create DWH database, or prefer to perform that manually? (Automatic, Manual) [Automatic]:
          Where is the Engine database located? (Local, Remote) [Local]: ENTER

          Setup can configure the local postgresql server automatically for the engine to run. This may conflict with existing applications.
          Would you like Setup to automatically configure postgresql and create Engine database, or prefer to perform that manually? (Automatic, Manual) [Automatic]: ENTER

          --== OVIRT ENGINE CONFIGURATION ==--

          Engine admin password: <--- INGRESAR PASSWORD
          Confirm engine admin password: <--- CONFIRMAR PASSWORD
          Application mode (Virt, Gluster, Both) [Both]: ENTER
          Use default credentials (admin@internal) for ovirt-p provider-ovn (Yes, No) [Yes]: Yes

          --== STORAGE CONFIGURATION ==--

          Default SAN wipe after delete (Yes, No) [No]: No

          --== PKI CONFIGURATION ==--

          Organization name for certificate [example.com]: ENTER

          --== APACHE CONFIGURATION ==--

          Setup can configure the default page of the web server to present the application home page. This may conflict with existing applications.
          Do you wish to set the application as the default page of the web server? (Yes, No) [Yes]: Yes

          Setup can configure apache to use SSL using a certificate issued from the internal CA.
          Do you wish Setup to configure that, or prefer to perform that manually? (Automatic, Manual) [Automatic]: ENTER

          --== SYSTEM CONFIGURATION ==--


          --== MISC CONFIGURATION ==--

          Please choose Data Warehouse sampling scale:
          (1) Basic
          (2) Full
          (1, 2)[1]: 1
          Use Engine admin password as initial Grafana admin password (Yes, No) [Yes]: Yes

          --== END OF CONFIGURATION ==--

          --== CONFIGURATION PREVIEW ==--

          Application mode                        : both
          Default SAN wipe after delete           : False
          Host FQDN                               : ovirt.example.com
          Firewall manager                        : firewalld
          Update Firewall                         : True
          Set up Cinderlib integration            : False
          Configure local Engine database         : True
          Set application as default page         : True
          Configure Apache SSL                    : True
          Engine database host                    : localhost
          Engine database port                    : 5432
          Engine database secured connection      : False
          Engine database host name validation    : False
          Engine database name                    : engine
          Engine database user name               : engine
          Engine installation                     : True
          PKI organization                        : example.com
          Set up ovirt-provider-ovn               : True
          Grafana integration                     : True
          DWH database host                       : localhost
          DWH database port                       : 5432
          DWH database secured connection         : False
          DWH database host name validation       : False
          DWH database name                       : ovirt_engine_history
          Grafana database user name              : ovirt_engine_history_grafana
          Configure WebSocket Proxy               : True
          DWH installation                        : True
          Configure local DWH database            : True
          Configure VMConsole Proxy               : True

          Please confirm installation settings (OK, Cancel) [OK]: OK o ENTER
[ INFO  ] Stage: Transaction setup
[ INFO  ] Stopping engine service
[ INFO  ] Stopping ovirt-fence-kdump-listener service
[ INFO  ] Stopping dwh service
[ INFO  ] Stopping vmconsole-proxy service
[ INFO  ] Stopping websocket-proxy service
[ INFO  ] Stage: Misc configuration (early)
[ INFO  ] Stage: Package installation
[ INFO  ] Stage: Misc configuration
[ INFO  ] Upgrading CA
[ INFO  ] Initializing PostgreSQL
[ INFO  ] Creating PostgreSQL 'engine' database
[ INFO  ] Configuring PostgreSQL
[ INFO  ] Creating PostgreSQL 'ovirt_engine_history' database
[ INFO  ] Configuring PostgreSQL
[ INFO  ] Creating CA: /etc/pki/ovirt-engine/ca.pem
[ INFO  ] Creating CA: /etc/pki/ovirt-engine/qemu-ca.pem
[ INFO  ] Updating OVN SSL configuration
[ INFO  ] Creating/refreshing DWH database schema
[ INFO  ] Setting up ovirt-vmconsole proxy helper PKI artifacts
[ INFO  ] Setting up ovirt-vmconsole SSH PKI artifacts
[ INFO  ] Configuring WebSocket Proxy
[ INFO  ] Creating/refreshing Engine database schema
[ INFO  ] Creating a user for Grafana
[ INFO  ] Creating/refreshing Engine 'internal' domain database schema
[ INFO  ] Creating default mac pool range
[ INFO  ] Adding default OVN provider to database
[ INFO  ] Adding OVN provider secret to database
[ INFO  ] Setting a password for internal user admin
[ INFO  ] Install selinux module /usr/share/ovirt-engine/selinux/ansible-runner-service.cil
[ INFO  ] Generating post install configuration file '/etc/ovirt-engine-setup.conf.d/20-setup-ovirt-post.conf'
[ INFO  ] Stage: Transaction commit
[ INFO  ] Stage: Closing up
[ INFO  ] Starting engine service
[ INFO  ] Starting dwh service
[ INFO  ] Starting Grafana service
[ INFO  ] Restarting ovirt-vmconsole proxy service

          --== SUMMARY ==--

[ INFO  ] Restarting httpd
          Please use the user 'admin@internal' and password specified in order to login
          Web access is enabled at:
              http://ovirt.example.com:80/ovirt-engine
              https://ovirt.example.com:443/ovirt-engine
          Internal CA 15:40:59:64:D5:C9:39:F2:23:89:41:71:2D:DB:03:B4:60:F0:3F:1F
          SSH fingerprint: SHA256:d948EGOxkthqdAHeVcfnZBqQGrThWeXjkIe8wtw1G7A
[WARNING] Less than 16384MB of memory is available
          Web access for grafana is enabled at:
              https://ovirt.example.com/ovirt-engine-grafana/
          Please run the following command on the engine machine ovirt.example.com, for SSO to work:
          systemctl restart ovirt-engine

          --== END OF SUMMARY ==--

[ INFO  ] Stage: Clean up
          Log file is located at /var/log/ovirt-engine/setup/ovirt-engine-setup-20200804130408-e92f29.log
[ INFO  ] Generating answer file '/var/lib/ovirt-engine/setup/answers/20200804130408-setup.conf'
[ INFO  ] Stage: Pre-termination
[ INFO  ] Stage: Termination
[ INFO  ] Execution of setup completed successfully
```

El firewall se configurará automáticamente abriendo los puertos necesarios para la administración de la plataforma oVirt Engine y también de la consola de las maquinas virtuales.  

Accedemos ahora a la plataforma web, abrimos un navegador y digitamos la URL:  

***[https://ovirt.example.com](https://ovirt.example.com)***

Ahora si podriamos ver la interfaz de administración de oVirt:  

![Pantalla Principal](/images/ovirt-4.4/ovirt-1.png)

Click en Portal de Administración, colocamos usuario: admin y contraseña (la que previamente configuramos en el setup):  

![Inicio de Sesión](/images/ovirt-4.4/ovirt-2.png)
![Principal](/images/ovirt-4.4/ovirt-3.png)

Hasta aquí ya hemos configurado el oVirt Engine, ahora vamos a configurar paso a paso el Hypervisor.  

### Paso 6: Configurar oVirt Node  

***

Vamos al menú Computo -> Centro de Datos  

![Principal](/images/ovirt-4.4/ovirt-4.png)

Una vez allí, creamos un nuevo centro de datos:  

Click en Aceptar con esa configuración.  

![Principal](/images/ovirt-4.4/ovirt-5.png)

Ahora vamos al menu Computo -> Clusteres  

![Principal](/images/ovirt-4.4/ovirt-6.png)

Creamos un nuevo cluster de datos, cambiando el Centro de Datos al que creamos anteriormente (en este caso **pve**):  

![Principal](/images/ovirt-4.4/ovirt-7.png)

Las opciones mas importantes a configurar son: Arquitectura de CPU (en este caso x86_64) y Tipo de CPU, como tengo un procesador AMD, voy a escoger la opción AMD EPYC (si tienes un procesador Intel debes cambiarlo a la familia a la que pertenezca el micro)  

Ahora vamos al menú Computo -> Hosts para configurar el host local  

![Principal](/images/ovirt-4.4/ovirt-8.png)

Click en Nuevo.  

![Principal](/images/ovirt-4.4/ovirt-9.png)

Tenemos que colocar el nombre del cluster donde va a estar ubicado, en este caso el que creamos anteriormente (***cluster1***), al ser el Hypervisor en el mismo host donde está instalado el oVirt Engine, debemos colocar como Nombre **localhost** y en Nombre de host **localhost** igual. En la autenticación escogemos la opción por contraseña de usuario root.  

![Principal](/images/ovirt-4.4/ovirt-10.png)

Nos va a indicar este mensaje, damos click en Aceptar.  

![Principal](/images/ovirt-4.4/ovirt-11.png)

A este punto, se van a instalar los paquetes necesarios para el correcto funcionamiento de oVirt Node. Si vamos al Dashboard y hacemos click en el icono de la campanita (esquina superior derecha) veremos el progreso de la instalación.  

Una vez finalizada la instalación, tendremos al host ejecutándose.  

![Principal](/images/ovirt-4.4/ovirt-12.png)

En caso que no llegar a activarse el host o que nos de un error de “no operacional”, tenemos que revisar que el tipo de CPU escogido sea el correcto para la arquitectura donde se esta corriendo el Hypervisor. Un ejemplo de esto escoger un procesador AMD en la configuracion cuando en realidad se trata de un Intel o viceversa.  

Como trobleshooting tendriamos que colocar en host en Mantenimiento, escogiendo el host de la lista, y dando click al boton Administracion, luego Mantenimiento, tal cual como la siguiente imagen:  

![Principal](/images/ovirt-4.4/ovirt-13.png)

### Paso 7: Creamos el almacenamiento de las imágenes de las VM e ISO  

***

Crearemos ahora el Dominio de Almacenamiento, previamente tenemos que crear las carpetas iso y data para almacenar tanto los instaladores como los discos virtuales. En la consola tecleamos:  

```bash
[root@ovirt ~]# mkdir -p /vz/{iso,data}
```

Ajustamos el dueño de las carpetas, en este caso vdsm, grupo kvm:  

```bash
[root@ovirt ~]# chown /vz -R vdsm:kvm
```

Comprobamos que se hayan realizado los cambios:  

```bash
[root@ovirt ~]# ls -ls /vz
total 8
4 drwxr-xr-x. 2 vdsm kvm 4096 aug 04 00:39 data
4 drwxr-xr-x. 2 vdsm kvm 4096 aug 04 14:39 iso
[root@ovirt ~]#
```

Vamos al menú Almacenamiento -> Dominios  

![Principal](/images/ovirt-4.4/ovirt-14.png)

Creamos el dominio Data.  

![Principal](/images/ovirt-4.4/ovirt-15.png)

Click en Aceptar.  

![Principal](/images/ovirt-4.4/ovirt-16.png)

Hacemos lo mismo con el dominio ISO.  

![Principal](/images/ovirt-4.4/ovirt-17.png)

Ya tendremos configurado el dominio de almacenamiento.  

![Principal](/images/ovirt-4.4/ovirt-18.png)

Ya con estos pasos tendriamos un host totalmente funcional para hacer pruebas.

Espero les haya gustado ¡Hasta la próxima!

Fuentes: ***[Installing oVirt as a self-hosted engine using the command line](https://www.ovirt.org/documentation/installing_ovirt_as_a_self-hosted_engine_using_the_command_line/)***
