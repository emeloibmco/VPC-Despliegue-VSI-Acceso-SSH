# VPC Despliegue VSI - Acceso SSH 🔒🔑

*IBM Cloud ™ Virtual Private Cloud (VPC)* es una red virtual que está vinculada a su cuenta de cliente en *IBM Cloud*, proporcionando una forma de administrar sus recursos informáticos, de almacenamiento y de red. Con esta herramienta usted puede definir y controlar una red virtual en partes aisladas lógicamente, donde puede ejecutar y dar soporte a sus aplicaciones de misión crítica, tolerantes a la nube y nativas de la nube. Adicionalmente, puede aprovisionar *Virtual Server Instances (VSI)* para *VPC* con un alto rendimiento de red. 

La presente guía está enfocada en el despliegue de una VSI Linux en *VPC*, junto con la respectiva configuración para el acceso mediante *SSH*. En esta documentación se indican los pasos que van desde la creación de la *VPC* hasta el acceso a la *VSI*.

<br />

## Índice  📰
1. [Pre-Requisitos](#Pre-Requisitos-pencil)
2. [Crear  VPC](#Crear-VPC-cloud)
3. [Crear subred](#Crear-subred-wrench)
4. [Configurar claves SSH](#Configurar-claves-SSH-closed_lock_with_key)
5. [Desplegar VSI en VPC](#Desplegar-VSI-en-VPC-computer)
6. [Acceder a la VSI mediante SSH](#Acceder-a-la-VSI-mediante-SSH-trophy)
7. [Realizar pruebas de ancho de banda entre 2 VSI con iperf](#Realizar-pruebas-de-ancho-de-banda-entre-2-VSI-con-iperf-hammer_and_wrench)
8. [Referencias](#Referencias-mag)
9. [Autores](#Autores-black_nib)
<br />

## Pre Requisitos :pencil:
* Contar con una cuenta en <a href="https://cloud.ibm.com/"> IBM Cloud </a>.
* Contar con un grupo de recursos específico para la implementación de los recursos.
<br />

## Crear VPC :cloud:
Para realizar el ejercicio lo primero que debe hacer es crear una *VPC* en su cuenta de *IBM Cloud*. Para ello, siga los pasos que se indican a continuación:

1. De click en el ```Menú de Navegación``` o ```Menú de Hamburguesa``` y seleccione la pestaña ```Infraestructura VPC```.

2. En la sección de ```Red``` seleccione la opción ```VPCs``` y posteriormente de click en el botón ```Crear```. Una vez le aparezca la ventana para la configuración y creación de la *VPC*, complete lo siguiente:

* ```Nombre```: asigne un nombre exclusivo para la *VPC*.
* ```Grupo de recursos```: seleccione el grupo de recursos en el cual va a trabajar.
* ```Ubicación```: seleccione la ubicación en la cual desea implementar la *VPC*.
* ```Grupo de seguridad predeterminado```: deje seleccionadas las opciones *Permitir SSH* y *Permitir ping*.
* ```Acceso clásico```: deje el campo SIN seleccionar.
* ```Prefijos de dirección predeterminados```: deje el campo SIN seleccionar, ya que posteriormente se creara la subred en la que se va a trabajar.

Cuando ya tenga todos los campos configurados de click en el botón ```Crear nube privada virtual```.

<p align="center"><img width="700" src="https://github.com/emeloibmco/VPC-Despliegue-VSI-Acceso-SSH/blob/main/Imagenes/vpc.gif"></p>

3. Espere unos minutos mientras la *VPC* aparece en estado disponible y asegúrese de tener seleccionada la región en la cual la implementó.
4. Una vez haya sido aprovisonada la VPC, de click en el nombre e ingrese a la pestaña ```Prefijos de dirección```. En dicha pestaña, de click en ```Crear``` e ingrese la dirección IP que desee junto con la máscara.

> NOTA: Puede utilizar la IP y máscara sugeridas en las subnets creadas por defecto cuando se estaba aprovisionando la VPC.
<p align="center"><img width="700" src="https://github.com/emeloibmco/VPC-Despliegue-VSI-Acceso-SSH/blob/main/Imagenes/prefijo.gif"></p>

<br />


## Crear subred :wrench:
El siguiente paso consiste en crear un Subred en la *VPC*. Para ello, en la sección de ```Red``` seleccione la opción ```Subredes``` y de click en el botón ```Crear```. Una vez le aparezca la ventana para la configuración y creación de la subred, complete lo siguiente:

* ```Nombre```: asigne un nombre exclusivo para la subred.
* ```Grupo de recursos```: seleccione el grupo de recursos en el cual va a trabajar (el mismo seleccionado en la creación de la *VPC*).
* ```Ubicación```: seleccione la ubicación en la cual desea implementar la subred (la misma seleccionada en la creación de la *VPC*).
* ```Nube privada virtual```: seleccione la *VPC* que creó anteriormente.
* Los demás parámetros no los modifique, deje los valores establecidos por defecto.

Cuando ya tenga todos los campos configurados de click en el botón ```Crear subred```.
<p align="center"><img width="700" src="https://github.com/emeloibmco/VPC-Despliegue-VSI-Acceso-SSH/blob/main/Imagenes/subnet.gif"></p>

6. Espere unos minutos mientras la subred aparece en estado disponible y asegúrese de tener seleccionada la región en la cual la implementó.

<br />


## Configurar claves SSH :closed_lock_with_key:
Para poder desplegar una *VSI* en *VPC* es necesario realizar la respectiva configuración para las claves *SSH*. En base a esto, realice lo siguiente:

1. Para generar una clave *SSH* acceda al *IBM Cloud Shell* y coloque el comando:
```
ssh-keygen -t rsa -C "user_id"
```

2. Al colocar el comando anterior, en la consola se pide que especifique la ubicación, en este caso oprima la tecla Enter para que se guarde en la ubicación sugerida. Posteriormente, cuando se pida la ```Passphrase``` coloque una constraseña que pueda recordar o guardela, ya que se utilizará más adelante.

3. Muévase con el comando ```cd .ssh``` a la carpeta donde están los archivos ```id_rsa.pub``` y ```id_rsa```. Estos archivos contienen las claves públicas y privadas respectivamente. 

4. Visualice la clave pública, ya que la necesitara para la creación de la *VSI*. Utilice el comando:
```
cat id_rsa.pub
```
> NOTA: Por defecto la clave empieza con ssh_rsa y termina con el user_ID. Copie la clave para emplearla más adelante.

<p align="center"><img width="700" src="https://github.com/emeloibmco/VPC-Despliegue-VSI-Acceso-SSH/blob/main/Imagenes/ssh.gif"></p>

<br />


## Desplegar VSI en VPC :computer:
Un vez ha configurado las claves *SSH* proceda con la creación de la *VSI* Linux en *VPC*. Complete los siguientes pasos:

1. En la sección de ```Computación``` seleccione la opción ```Instancias de Servidor Virtual``` y posteriormente de click en el botón ```Crear```. Una vez le aparezca la ventana para la configuración y creación de la *VSI*, complete lo siguiente:

* ```Nombre```: asigne un nombre exclusivo para la *VSI*.
* ```Grupo de recursos```: seleccione el grupo de recursos en el cual va a trabajar (el mismo seleccionado en la creación de la *VPC*).
* ```Ubicación```: seleccione la ubicación en la cual desea implementar la subred (la misma seleccionada en la creación de la *VPC*).
* ```Tipo de servidor virtual```: seleccione la opción **Público**.
* ```Sistema operativo```: seleccione la opción **Ubuntu Linux**.
* ```Perfil```: deje seleccionado el perfil que viene por defecto (**Equilibrado | bx2-2x8**).
* ```Claves SSH```: de click en el botón ```Clave nueva +```, asigne un nombre exclusivo para su clave *SSH*, seleccione el grupo de recursos y la ubicación y finalmente en **Clave pública** coloque la clave copiada en el ítem 3 del paso [Configurar claves SSH](#Configurar-claves-SSH-closed_lock_with_key). Posteriormente, de click en el botón ```Añadir clave SSH```.
* ```Nube privada virtual```: seleccione la *VPC* creada anteriormente.
* Los demás parámetros no los modifique, deje los valores establecidos por defecto.

Cuando ya tenga todos los campos configurados de click en el botón ```Crear instancia de servidor virtual```.

<p align="center"><img width="700" src="https://github.com/emeloibmco/VPC-Despliegue-VSI-Acceso-SSH/blob/main/Imagenes/vsi.gif"></p>

2. Espere unos minutos mientras la *VSI* aparece en estado disponible y asegúrese de tener seleccionada la región en la cual la implementó.

<br />

## Acceder a la VSI mediante SSH :trophy:
Para acceder a la *VSI* mediante *SSH* realice lo siguiente:

1. Configure la IP Flotante. Para ello, haga click en la *VSI* implementada y en la sección de ```Interfaces de Red``` seleccione la opción ```editar```. 

2. En la opción ```Dirección IP flotante``` seleccione la opción ```Reservar IP flotante```. Luego, de click en ```Guardar```. Después de esto debe poder visualizar la IP flotante de la *VSI* en la sección de ```Interfaces de Red```.
<p align="center"><img width="700" src="https://github.com/emeloibmco/VPC-Despliegue-VSI-Acceso-SSH/blob/main/Imagenes/ipflotante.gif"></p>

3. En *IBM Cloud Shell* cambie la región y el grupo de recursos mediante los comandos:
```
ibmcloud target -r <REGION>
ibmcloud target -g <GRUPO_RECURSOS>
```
4. Para visualizar en la linea de comandos, si la VSI ha sido creada correctamente, ingrese el siguiente comando:
```
ibmcloud is instances
```
<p align="center"><img width="700" src="https://github.com/emeloibmco/VPC-Despliegue-VSI-Acceso-SSH/blob/main/Imagenes/verinstancia.gif"></p>

5. Conéctese a la *VSI* usando la clave privada y la IP flotante reservada anteriormente. Para ello utilice el comando: 
```
ssh -i ./id_rsa root@<ip_flotante>
```

6.  Al colocar el comando anterior, en la consola se pide una confirmación para seguir con el acceso, ingrese ```yes```. Posteriormente, ingrese la ```Passphrase``` elegida anteriormente.

7. Si desea asignar una nueva contraseña utilice el comando:
```
passwd 
```
<p align="center"><img width="700" src="https://github.com/emeloibmco/VPC-Despliegue-VSI-Acceso-SSH/blob/main/Imagenes/pass.PNG"></p>
<br />

8. Configuración adicional para acceder a la VSI por medio de SSH 

Para acceder a la vsi por medio de la contraseña que estableció, debe realizar una configuración adicional en la *vsi*:
* Para acceder al archivo de configuración de conexión por ssh, ejecute el siguiente comando:
```
sudo nano /etc/ssh/sshd_config 
```
* En la sección *Authentication* asegurese que todos los parámetros se hayan establecido de la siguiente manera:
<p align="center"><img width="700" src="https://github.com/emeloibmco/VPC-Despliegue-VSI-Acceso-SSH/blob/main/Imagenes/ssh_linux.PNG"></p>

Una vez configurado, guarde los cambios con ```Ctrl+S``` y regrese a la linea de comandos con ```Ctrl+X```.

* Por último reinicie los servicios de red con el siguiente comando:
```
systemctl restart sshd.service 
```
<br />

> NOTA: Después de realizar la configuración, si desea acceder nuevamente a la *VSI* mediante *SSH* lo único que debe hacer es ingresar el comando ```ssh root@<ip_flotante>``` y posteriormente la contraseña establecida.
<br />

## Realizar pruebas de ancho de banda entre 2 VSI con iperf :hammer_and_wrench:
El termino *latencia* corresponde al tiempo que se tarda la transmisión de información a través de una red. *IBM Cloud* cuenta con la herramienta <a href="http://lg.softlayer.com/">lg.softlayer.com</a>, que permite visualizar datos sobre latencia de red entre servidores en distintas ubicaciones. En la siguiente imagen se presenta la tabla que contiene los datos. 
<br />

<p align="center"><img width="700" src="https://github.com/emeloibmco/VPC-Despliegue-VSI-Acceso-SSH/blob/main/Imagenes/latency.png"></p>
<br />

Para este caso, si se analizan los servidores en ubicaciones como Dallas y Londres se obtiene que la latencia de red tiene un valor de ```108 ms```. Es importante aclarar que los datos presentados en la tabla son estáticos y no una representación en tiempo real.
<br />

Por otro lado, el *ancho de banda* corresponde a la cantidad de datos que se pueden transmitir por segundo (medido en este caso en Mbits/s). Para realizar esta medición se utiliza el comando ```iperf```, que es una herramienta de la línea de comandos usada en el diagnóstico de problemas de velocidad de red. Este comando mide la capacidad máxima de procesamiento de red que puede manejar un servidor. Es particularmente útil cuando se experimentan problemas de velocidad en la red, debido a que se puede utilizar para determinar cuál servidor es incapaz de llegar al rendimiento máximo. Para este ejercicio se implementan 2 VSI ubicadas en Dallas y Londres y posteriormente se realiza la respectiva prueba para medir el ancho de banda entre ambos servidores mediante el comando ```iperf```, tal y como se presenta en los siguientes pasos.

<br />

### a. Crear VPC, subred y VSI en Dallas y Londres
Para realizar el test, en primero lugar debe implementar:
* Una *VPC* en Dallas y una *VPC* en Londres. Tomar como guía el paso [Crear  VPC](#Crear-VPC-cloud).
* Una subred en cada *VPC* (Dallas y Londres). Tomar como guía el paso [Crear subred](#Crear-subred-wrench).
* Una *VSI* con SO CentOS en Dallas.

<p align="center"><img width="700" src="https://github.com/emeloibmco/VPC-Despliegue-VSI-Acceso-SSH/blob/main/Imagenes/vsi_dallas.gif"></p>
<br />

* Una *VSI* con SO CentOS en Londres. 

<p align="center"><img width="700" src="https://github.com/emeloibmco/VPC-Despliegue-VSI-Acceso-SSH/blob/main/Imagenes/Crear%20VSI%20Londres.gif"></p>
<br />


Recuerde el paso sobre como [Configurar claves SSH](#Configurar-claves-SSH-closed_lock_with_key) de forma previa para crear sus instancias. 

<br />

### b. Configurar archivos y acceder a VSI Dallas y VSI Londres
Como se comento en [configuración adicional para acceder a la VSI por medio de SSH ](#Configuración-adicional-para-acceder-a-la-VSI-por-medio-de-SSH), para acceder por medio de SSH a las *VSI* de Londres y Dallas, es necesario realizar una configuración previa en las *VSI*. Debido a que las máquinas aprovisionadas tienen sistema operativo *CentOS*, debe previamente instalar el comando ```nano``` ejecutando el siguiente comando:

```
yum install nano
```
Los demás comandos se aplican de la misma forma que en la máquina Ubuntu.


<p align="center"><img width="700" src="https://github.com/emeloibmco/VPC-Despliegue-VSI-Acceso-SSH/blob/main/Imagenes/configssh.gif"></p>

Una vez se realice la configuración podrá conectarse por medio de *SSH* únicamente ingresando el ```password``` configurado.

<p align="center"><img width="700" src="https://github.com/emeloibmco/VPC-Despliegue-VSI-Acceso-SSH/blob/main/Imagenes/ingreso.PNG"></p>

<br />

### c. Instalar comando iperf3 en cada VSI

Actualmente hay dos ramas independientes de ```iPerf``` que se desarrollan en paralelo: ```iPerf2``` y ```iPerf3```. La funcionalidad de estas herramientas es en su mayoría compatible, pero utilizan diferentes puertos de red de forma predeterminada. En ```iPerf1/2``` es 5001, en ```iPerf3``` es 5201.

Las diferencias no son tan significativas, por lo que no es necesario utilizar una versión específica de ```iPerf```, en este ejercicio usaremos ```iperf3```.

Para instalar ```iperf3``` en las VSI *CentOS*, ejecute:

```
yum update
dnf install iperf3
```

<p align="center"><img width="1000" src="https://github.com/emeloibmco/VPC-Despliegue-VSI-Acceso-SSH/blob/main/Imagenes/iperf.PNG"></p>

Una vez termine de instalar, podrá utilizar el comando iperf3 en las *VSI*.
<br />

### d. Configurar reglas en VSI
<br />

### e. Realizar test de ancho de banda con iperf
<br />

## Referencias :mag:
* <a href="https://cloud.ibm.com/docs/vpc?topic=vpc-ssh-keys">SSH Keys</a>.
* <a href="https://cloud.ibm.com/docs/vpc?topic=vpc-creating-virtual-servers">Creating virtual server instances</a>.
* <a href="https://cloud.ibm.com/docs/vpc?topic=vpc-vsi_is_connecting_linux">Connecting to Linux instances</a>.
<br />


## Autores :black_nib:
Equipo IBM Cloud Tech Sales Colombia.
<br />
