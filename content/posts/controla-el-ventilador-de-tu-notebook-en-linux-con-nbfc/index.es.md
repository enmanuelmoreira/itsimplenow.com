---
title: "Controla el Ventilador de tu Notebook en Linux con NBFC"
date: 2021-07-06
lastmod: 2021-07-06
author: Enmanuel Moreira
description: "NBFC es un software multiplataforma escrito en Mono, que nos permite controlar la velocidad de los ventiladores del CPU y de la GPU de nuestro Notebook. Tiene un poderoso sistema de configuración, el cual permite ajustarlo a diferentes modelos de notebook"
draft: false

resources:
  - name: "featured-image"
    src: "featured-image.jpg"

categories: ["Linux", "Tutorial"]
tags: ["linux", "fan-control", "tutorial", "notebooks"]

lightgallery: true
---

<!--more-->

[NBFC](https://www.github.com/hirschmann/nbfc) es un software multiplataforma escrito en Mono, que nos permite controlar la velocidad de los ventiladores del CPU y de la GPU de nuestro Notebook. Tiene un poderoso sistema de configuracion, el cual permite ajustarlo a diferentes modelos de notebook.  

En esta guía vamos a instalar nbfc para poder controlar la velocidad de los ventiladores de nuestro notebook.  

{{< admonition tip >}}
Si tienes Secure Boot habilitado en la Notebook, es probable que nbfc no inicie, por lo que tienes que ir a la BIOS de tu máquina y deshabilitarlo.
{{< /admonition >}}

## Requisitos

***

Debemos instalar Mono para poder compilar las fuentes.  

### Ubuntu 20.04 LTS

***

Agregamos el repositorio de Mono para obtener los paquetes más recientes e instalamos Mono de una vez:  

```bash
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
echo "deb https://download.mono-project.com/repo/ubuntu stable-focal main" | sudo tee /etc/apt/sources.list.d/mono-official-stable.list
sudo apt update
sudo apt install mono-complete
```

### Fedora 34 / CentOS / RHEL / Rocky / Alma Linux

***

Instalamos Mono desde los repositorios:  

```bash
sudo dnf install mono mono-devel
```

{{< admonition important >}}
**IMPORTANTE**: Las versiones más recientes de los sistemas operativos basados en RedHat han bloqueado la manera en que los certificados firmados son gestionados y puede ser que rompan buena parte del script de compilación si se ejecuta con permisos de root (sudo). Recomiendo sincronizar los certificados antes de compilar:  

```bash
sudo cert-sync /etc/pki/tls/certs/ca-bundle.crt
```

{{< /admonition >}}

## Compilando nbfc

***

### Docker (Recomendado)

***

En caso de que no quieras instalar las dependencias de desarrollo para compilar nbfc, podemos hacerlo a través de Docker, del cual hice una guía de instalación que puedes consultar [aquí](https://www.itsimplenow.com/como-instalar-docker-en-linux/)  

Clonamos el repositorio de nbfc en nuestro host:  

```bash
git clone https://github.com/hirschmann/nbfc.git nbfc
```

Iniciamos la imagen de Docker de Mono y entramos directamente al contenedor:  

```bash
docker run -it -v $(pwd)/nbfc:/nbfc mono:latest bash
```

Una vez dentro del contenedor, vamos a la carpeta de nbfc, y ejecutamos el script que compilará nbfc:  

```bash
cd /nbfc
./build.sh
```

### Ubuntu 20.04 o RHEL

***

Una vez tengamos instalados los requisitos, descargamos las fuentes desde el sitio oficila de nbfc en Github:  

```bash
git clone https://github.com/hirschmann/nbfc.git nbfc
```

Entramos a la carpeta nbfc y ejecutamos el script de compilacion:

```bash
cd /nbfc
./build.sh
```

Una vez que termine la compilacion, tendremos nbfc listo para instalar en nuestro computador.

### Manjaro

***

Se puede instalar desde AUR con yay:

```bash
yay -S nbfc
```

## Instalando nbfc

Una vez que termine la compilación, tendremos nbfc listo para instalar en nuestro computador.  

Creamos la carpeta nbfc en /opt:  

```bash
sudo mkdir /opt/nbfc
```

Copiamos el contenido de la carpeta a /opt/nbfc:  

```bash
sudo cp -r ~/nbfc/Linux/bin/Release /opt/nbfc/
```

Copiamos los archivos para arrancar nbfc desde el inicio del computador con systemd:  

```bash
sudo cp ~/nbfc/Linux/{nbfc.service,nbfc-sleep.service} /etc/systemd/system/
```

Habilitamos el servicio y lo iniciamos:  

```bash
sudo systemctl enable nbfc --now
```

## Aplicando la Configuración a Nuestra Notebook

Lo ultimo que nos queda por hacer es aplicar el patrón de configuración dependiendo de nuestro modelo de notebook. Se encuentra en la ruta /opt/nbfc/Configs, y los aplicamos de esta manera:

```bash
mono nbfc.exe config --apply "Nombre del archivo de configuracion sin la extension"
```

En el caso que no encontremos un archivo de configuración parecido, podemos chequear en los **[Pull Requests](https://github.com/hirschmann/nbfc/pulls)** del proyecto si alguien hizo uno 😉, en mi caso la de la Acer Nitro 5 AN515-43, copio el contenido, creo un nuevo archivo en /opt/nbfc/Configs y lo aplico.

Acer Nitro 5 AN515-43.xml

```xml
<?xml version="1.0"?>
<FanControlConfigV2 xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <NotebookModel>Acer Nitro 5 AN515-43(AMD GPU)</NotebookModel>
  <Author>Author</Author>
  <EcPollInterval>3000</EcPollInterval>
  <ReadWriteWords>true</ReadWriteWords>
  <CriticalTemperature>90</CriticalTemperature>
  <FanConfigurations>
    <FanConfiguration>
      <ReadRegister>19</ReadRegister>
      <WriteRegister>55</WriteRegister>
      <MinSpeedValue>0</MinSpeedValue>
      <MaxSpeedValue>100</MaxSpeedValue>
      <IndependentReadMinMaxValues>true</IndependentReadMinMaxValues>
      <MinSpeedValueRead>0</MinSpeedValueRead>
      <MaxSpeedValueRead>6122</MaxSpeedValueRead>
      <ResetRequired>true</ResetRequired>
      <FanSpeedResetValue>50</FanSpeedResetValue>
      <FanDisplayName>AMD Ryzen 5 3550H</FanDisplayName>
      <TemperatureThresholds>
        <TemperatureThreshold>
          <UpThreshold>0</UpThreshold>
          <DownThreshold>0</DownThreshold>
          <FanSpeed>0</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>40</UpThreshold>
          <DownThreshold>40</DownThreshold>
          <FanSpeed>30</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>42</UpThreshold>
          <DownThreshold>41</DownThreshold>
          <FanSpeed>32</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>44</UpThreshold>
          <DownThreshold>43</DownThreshold>
          <FanSpeed>34</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>48</UpThreshold>
          <DownThreshold>46</DownThreshold>
          <FanSpeed>36</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>52</UpThreshold>
          <DownThreshold>50</DownThreshold>
          <FanSpeed>38</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>54</UpThreshold>
          <DownThreshold>53</DownThreshold>
          <FanSpeed>41</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>58</UpThreshold>
          <DownThreshold>56</DownThreshold>
          <FanSpeed>44</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>62</UpThreshold>
          <DownThreshold>60</DownThreshold>
          <FanSpeed>47</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>64</UpThreshold>
          <DownThreshold>63</DownThreshold>
          <FanSpeed>50</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>68</UpThreshold>
          <DownThreshold>66</DownThreshold>
          <FanSpeed>55</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>72</UpThreshold>
          <DownThreshold>70</DownThreshold>
          <FanSpeed>61</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>74</UpThreshold>
          <DownThreshold>73</DownThreshold>
          <FanSpeed>68</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>78</UpThreshold>
          <DownThreshold>76</DownThreshold>
          <FanSpeed>76</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>82</UpThreshold>
          <DownThreshold>80</DownThreshold>
          <FanSpeed>85</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>84</UpThreshold>
          <DownThreshold>83</DownThreshold>
          <FanSpeed>94</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>88</UpThreshold>
          <DownThreshold>86</DownThreshold>
          <FanSpeed>100</FanSpeed>
        </TemperatureThreshold>
      </TemperatureThresholds>
      <FanSpeedPercentageOverrides />
    </FanConfiguration>
    <FanConfiguration>
      <ReadRegister>21</ReadRegister>
      <WriteRegister>58</WriteRegister>
      <MinSpeedValue>0</MinSpeedValue>
      <MaxSpeedValue>100</MaxSpeedValue>
      <IndependentReadMinMaxValues>true</IndependentReadMinMaxValues>
      <MinSpeedValueRead>0</MinSpeedValueRead>
      <MaxSpeedValueRead>6122</MaxSpeedValueRead>
      <ResetRequired>true</ResetRequired>
      <FanSpeedResetValue>50</FanSpeedResetValue>
      <FanDisplayName>AMD Radeon RX560X</FanDisplayName>
      <TemperatureThresholds>
        <TemperatureThreshold>
          <UpThreshold>0</UpThreshold>
          <DownThreshold>0</DownThreshold>
          <FanSpeed>0</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>40</UpThreshold>
          <DownThreshold>40</DownThreshold>
          <FanSpeed>22</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>42</UpThreshold>
          <DownThreshold>41</DownThreshold>
          <FanSpeed>24</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>44</UpThreshold>
          <DownThreshold>43</DownThreshold>
          <FanSpeed>26</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>48</UpThreshold>
          <DownThreshold>46</DownThreshold>
          <FanSpeed>28</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>52</UpThreshold>
          <DownThreshold>50</DownThreshold>
          <FanSpeed>30</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>54</UpThreshold>
          <DownThreshold>53</DownThreshold>
          <FanSpeed>34</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>58</UpThreshold>
          <DownThreshold>56</DownThreshold>
          <FanSpeed>38</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>62</UpThreshold>
          <DownThreshold>60</DownThreshold>
          <FanSpeed>42</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>64</UpThreshold>
          <DownThreshold>63</DownThreshold>
          <FanSpeed>46</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>68</UpThreshold>
          <DownThreshold>66</DownThreshold>
          <FanSpeed>52</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>72</UpThreshold>
          <DownThreshold>70</DownThreshold>
          <FanSpeed>59</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>74</UpThreshold>
          <DownThreshold>73</DownThreshold>
          <FanSpeed>67</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>78</UpThreshold>
          <DownThreshold>76</DownThreshold>
          <FanSpeed>76</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>82</UpThreshold>
          <DownThreshold>80</DownThreshold>
          <FanSpeed>86</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>84</UpThreshold>
          <DownThreshold>83</DownThreshold>
          <FanSpeed>96</FanSpeed>
        </TemperatureThreshold>
        <TemperatureThreshold>
          <UpThreshold>88</UpThreshold>
          <DownThreshold>86</DownThreshold>
          <FanSpeed>100</FanSpeed>
        </TemperatureThreshold>
      </TemperatureThresholds>
      <FanSpeedPercentageOverrides />
    </FanConfiguration>
  </FanConfigurations>
  <RegisterWriteConfigurations>
    <RegisterWriteConfiguration>
      <WriteMode>Set</WriteMode>
      <WriteOccasion>OnInitialization</WriteOccasion>
      <Register>34</Register>
      <Value>12</Value>
      <ResetRequired>true</ResetRequired>
      <ResetValue>4</ResetValue>
      <ResetWriteMode>Set</ResetWriteMode>
      <Description>CPU fan manual mode</Description>
    </RegisterWriteConfiguration>
    <RegisterWriteConfiguration>
      <WriteMode>Set</WriteMode>
      <WriteOccasion>OnInitialization</WriteOccasion>
      <Register>33</Register>
      <Value>48</Value>
      <ResetRequired>true</ResetRequired>
      <ResetValue>16</ResetValue>
      <ResetWriteMode>Set</ResetWriteMode>
      <Description>GPU fan manual mode</Description>
    </RegisterWriteConfiguration>
    <RegisterWriteConfiguration>
      <WriteMode>Set</WriteMode>
      <WriteOccasion>OnInitialization</WriteOccasion>
      <Register>16</Register>
      <Value>0</Value>
      <ResetRequired>true</ResetRequired>
      <ResetValue>1</ResetValue>
      <ResetWriteMode>Set</ResetWriteMode>
      <Description>CoolBoost off</Description>
    </RegisterWriteConfiguration>
  </RegisterWriteConfigurations>
</FanControlConfigV2>
```

Ejemplo:  

```bash
mono nbfc.exe config --apply "Acer Nitro 5 AN515-43"
```

Iniciamos nbfc y comprobamos el estatus actual:  

```bash
mono nbfc.exe start
mono nbfc.exe status --all
```

Espero les haya gustado este tutorial, ¡hasta la próxima!

***

## Apoya este Proyecto!!!

Si te pareció útil este artículo y el proyecto en general, considera brindarme un café :)

<style>.bmc-button img{height: 34px !important;width: 35px !important;margin-bottom: 1px !important;box-shadow: none !important;border: none !important;vertical-align: middle !important;}.bmc-button{padding: 7px 15px 7px 10px !important;line-height: 35px !important;height:51px !important;text-decoration: none !important;display:inline-flex !important;color:#ffffff !important;background-color:#5F7FFF !important;border-radius: 8px !important;border: 1px solid transparent !important;font-size: 24px !important;letter-spacing: 0.6px !important;box-shadow: 0px 1px 2px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;margin: 0 auto !important;font-family:'Cookie', cursive !important;-webkit-box-sizing: border-box !important;box-sizing: border-box !important;}.bmc-button:hover, .bmc-button:active, .bmc-button:focus {-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;text-decoration: none !important;box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;opacity: 0.85 !important;color:#ffffff !important;}</style><link href="https://fonts.googleapis.com/css?family=Cookie" rel="stylesheet"><a class="bmc-button" target="_blank" href="https://www.buymeacoffee.com/enmanuelmoreira"><img src="https://cdn.buymeacoffee.com/buttons/bmc-new-btn-logo.svg" alt="Buy me a coffee"><span style="margin-left:5px;font-size:24px !important;">Buy me a coffee</span></a>
