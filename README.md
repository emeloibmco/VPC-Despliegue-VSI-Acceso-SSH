# VPC-Despliegue-VSI---Acceso-SSH 🔒🔑

*IBM Cloud ™ Virtual Private Cloud (VPC)* es una red virtual que está vinculada a su cuenta de cliente en *IBM Cloud*, proporcionando una forma de administrar sus recursos informáticos, de almacenamiento y de red. Con esta herramienta usted puede definir y controlar una red virtual en partes aisladas lógicamente, donde puede ejecutar y dar soporte a sus aplicaciones de misión crítica, tolerantes a la nube y nativas de la nube. Adicionalmente, puede aprovisionar *Virtual Server Instances (VSI)* para *VPC* con un alto rendimiento de red. 

La presente guía está enfocada en el despliegue de una VSI Linux en *VPC*, junto con la respectiva configuración para el acceso mediante *SSH*. En esta documentación se indican los pasos que van desde la creación de la *VPC* hasta el acceso a la *VSI*.

<br />

## Índice  📰
1. [Pre-Requisitos](#Pre-Requisitos-pencil)
2. [Crear nueva VPC](#Crear-nueva-VPC-cloud)
3. [Configurar claves SSH](#Configurar-claves-SSH-closed_lock_with_key)
4. [Desplegar VSI en VPC](#Desplegar-VSI-en-VPC-computer)
5. [Acceder a la VSI mediante SSH](#Acceder-a-la-VSI-mediante-SSH-trophy)
6. [Autores](#Autores-black_nib)
<br />

## Pre Requisitos :pencil:
* Contar con una cuenta en <a href="https://cloud.ibm.com/"> IBM Cloud </a>.
* Contar con un grupo de recursos específico para la implementación de la *VPC*.
<br />

## Crear nueva VPC :cloud:
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

3. Espere unos minutos mientras la *VPC* aparece en estado disponible y asegúrese de tener seleccionada la región en la cual la implementó.

4. Posteriormente, en la misma sección de ```Red``` seleccione la opción ```Subredes``` y de click en el botón ```Crear```. Una vez le aparezca la ventana para la configuración y creación de la subred, complete lo siguiente:

* ```Nombre```: asigne un nombre exclusivo para la subred.
* ```Grupo de recursos```: seleccione el grupo de recursos en el cual va a trabajar (el mismo seleccionado en la creación de la *VPC*).
* ```Ubicación```: seleccione la ubicación en la cual desea implementar la subred (la misma seleccionada en la creación de la *VPC*).
* ```Nube privada virtual```: seleccione la *VPC* que creó anteriormente.
* Los demás parámetros no los modifique, deje los valores establecidos por defecto.

Cuando ya tenga todos los campos configurados de click en el botón ```Crear subred```.

5. Espere unos minutos mientras la subred aparece en estado disponible y asegúrese de tener seleccionada la región en la cual la implementó.

<br />

## Configurar claves SSH :closed_lock_with_key:
Para poder desplegar una *VSI* en *VPC* es necesario realizar la respectiva configuración para las claves *SSH*. En base a esto, realice lo siguiente:

1. Para generar una clave *SSH* acceda al *IBM Cloud Shell* y coloque el comando:
```
ssh-keygen -t rsa -C "user_ID"
```
> NOTA: Recuerde reemplazar el user_ID con su respectivo ID de Usuario.

2. Muévase con el comando ```cd .ssh``` a la carpeta donde están los archivos ```id_rsa.pub``` y ```id_rsa```. Estos archivos contienen las claves públicas y privadas respectivamente. 

3. Visualice la clave pública, ya que la necesitara para la creación de la *VSI*. Utilice el comando:
```
cat id_rsa.pub
```
> NOTA: Por defecto la clave empieza con ssh_rsa y termina con el user_ID. Copie la clave para emplearla más adelante.


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

2. Espere unos minutos mientras la *VSI* aparece en estado disponible y asegúrese de tener seleccionada la región en la cual la implementó.

<br />

## Acceder a la VSI mediante SSH :trophy:
<br />

## Autores :black_nib:
Equipo IBM Cloud Tech Sales Colombia.
<br />
