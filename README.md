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

2. En la sección de ```Red``` seleccione la opción ```VPCs``` y posteriormente de click en el botón ```Crear```.

3. Una vez le aparezca la ventana para la configuración y creación de su *VPC*, complete lo siguiente:

* ```Nombre```: asigne un nombre exclusivo para su *VPC*.
* ```Grupo de Recursos```: seleccione el grupo de recursos en el cual va a trabajar.
* ```Región```: seleccione la región en la cual desea implementar su *VPC*.
* ```Grupo de seguridad predeterminado```: deje seleccionadas las opciones *Permitir SSH* y *Permitir ping*.
* ```Acceso clásico```: deje el campo SIN seleccionar.
* ```Prefijos de dirección predeterminados```: deje el campo SIN seleccionar, ya que posteriormente se creara la subred en la que se va a trabajar.

Cuando ya tenga todos los campos configurados de click en el botón ```Crear Nube Privada Virtual```.

4. Espere unos minutos mientras su *VPC* aparece en estado disponible y asegúrese de tener seleccionada la región en la cual la implementó.
<br />

## Configurar claves SSH :closed_lock_with_key:
<br />

## Desplegar VSI en VPC :computer:
<br />

## Acceder a la VSI mediante SSH :trophy:
<br />

## Autores :black_nib:
Equipo IBM Cloud Tech Sales Colombia.
<br />
