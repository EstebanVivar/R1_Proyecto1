# Proyecto 1

#### Grupo 10

| Nombre                        | Carne     |
| ----------------------------- | --------- |
| Ronald Alexandro Castro Perez | 201800699 |
| José Ottoniel Sincal Guitzol  | 201801375 |
| Carlos Esteban Vivar Torres   | 201801597 |



## Indice

* [Requisitos y Compatibiidad](#req)
  * [Compatibilidad](#comp)
  * [Requisitos GNS3](#gns3)
  * [Detalles de los equipos utilizados](#det)
    * [Topología 1](#t1)
    * [Topología 2](#t2)
    * [Topología 3](#t3)
* [Comandos de configuración](#conf)
  * [Configuración de VLAN's](#vlan)
  * [Configuración de interfaces en modo Trunk](#trunk)
  * [Configuración de interfaces en modo Access](#access)
  * [Configuración de VTP](#vtp)
  * [Configuración de STP](#stp)

<div id="req" />

## Requisitos y Compatibilidad

GNS3 es una plataforma que permite simular topologías de red con imágenes de vendors como Cisco y Juniper, entre otros. A continuación se muestran los requerimientos para la instalación del software.

<div id="comp" />

### Compatibilidad

* Windows 7 SP1 (64 bit)
* Windows 8 (64 bit)
* Windows 10 (64 bit)
* Windows Server 2012 (64 bit)
* Windows Server 2016 (64 bit)
* Distribuciones basadas en Ubuntu (solo 64 bits)
* Distribuciones basadad en Debian (solo 64 bits)

<div id="gns3" />

### Requisitos de GNS3

| Item              | Requisito                                                    |
| ----------------- | ------------------------------------------------------------ |
| Sistema Operativo | Los mencionados en el apartado anterior                      |
| Procesador        | 2 o más núcles lógicos                                       |
| Virtualización    | Se requieren extensiones de virtualización. Es posible que deba habilitar esto a través del BIOS de su computadora. |
| Memoria           | 4 GB RAM                                                     |
| Espacio en disco  | 1GB de espacio disponible (la instalación es < 200MB).       |
| Otros             | Es posible que necesite almacenamiento adicional para su sistema operativo e imágenes de los equipos. |

<div id="det" />

### Detalle de los equipos utilizados

<div id="t1" />

#### Topología 1 

- **Sistema Operativo**: Windows 10 (64 bits)
- **Procesador**: Intel® Core™ i7
- **Memoria**: 16 GB RAM
- **Disco**: 2 TB 

<div id="t2" />

#### Topología 2

- **Sistema Operativo**: Zorin OS 16 (basada en Ubuntu)
- **Procesador**: Intel® Core™ i5-7200U CPU @ 2.50GHz × 4
- **Memoria**: 8 GB RAM
- **Disco**: 1 TB 

<div id="t3" />

#### Topología 3

- **Sistema Operativo**: Windows 10 (64 bits)
- **Procesador**: Intel® Core™ i5-10300H
- **Memoria**: 8 GB RAM
- **Disco**: 256 GB SSD NVME



<div id="conf" />

## Comandos de configuración

<div id="vlan" />

### Configuración de VLAN's

Esto solo se hace en el switch que el cual se configurará en *VTP Server*

1. Entrar en modo de configuracion al switch

   ```
   > configure terminal
   ```

   

2. Crear las vlan y asignarle su repectivo nombre de idenficación

   ```
   > vlan [n]   		## en donde n es el número que se le asignará al a vlan
   > name [nombre]    	## sustituir por el nombre que se le dará a la vlan
   > exit				## para salir de la configuración de dicha vlan
   ```

Repetir el paso 2 las veces necesarias para la creación de distintas vlan's

<div id="trunk" />

### Configuración de interfaces en modo Trunk

Este comando se ejecuta en todos los switches. Cabe mencionar que se debe efectuar en las interfaces que conectan con otros switches. (switch - switch)

1. Entrar en modo de configuracion al switch

   ```
   > configure terminal
   ```

   

2. Poner las interfaces por que conectan a otro switch en modo trunk

   ```
   > interface fa[int]		## en donde int es la interfaz que se desea configurar ej: 1/1
   > switchport mode trunk	## activa el modo trunk en dicha interfaz del switch
   > switchport trunk allowed vlan 1,110,210,310,410,1002-1005 ## activa la comunicacion de las vlan's en el switch
   > exit
   ```

Repetir el paso 2 las veces necesarias para la configuracion del modo trunk en los diferentes puertos. Si se desea optimizar el paso 2 se puede realizar de la siguiente manera.

```
> interface range fa[rango]		## en donde rango correspnde a un rango de interfaces que se desea configurar ej: 1/0-2
> switchport mode trunk	## activa el modo trunk en dicho rango de interfaces del switch
> switchport trunk allowed vlan 1,110,210,310,410,1002-1005 ## activa la comunicacion de las vlan's en el switch
> exit
```

Esto ultimo solo se debe generar una sola vez.

<div id="access" />

### Configuración de interfaces en modo Access

Se configura en todo switch que tenga conectado una vpc a una de sus interfaces.

1. Entrar en modo de configuracion al switch

   ```
   > configure terminal
   ```

   

2. Poner las interfaces por que conectan a un vpc en modo access

   ```
   > interface fa[int]		## en donde int es la interfaz que se desea configurar ej: 1/1
   > switchport mode access	 ## activa el modo access en dicha interfaz del switch
   > switchport access vlan [n] ## asignar acceso a la vlan que se desea dar conexión
   > exit
   ```

Repetir el paso 2 las veces necesarias para la configuracion del modo access en los diferentes puertos. Si se desea optimizar el paso 2 se puede realizar de la siguiente manera.

```
> interface range fa[rango]		## en donde rango correspnde a un rango de interfaces que se desea configurar ej: 1/0-2
> switchport mode access	## activa el modo access en dicho rango de interfaces del switch
> switchport access vlan [n] ## asignar acceso a la vlan que se desea dar conexión
> exit
```

<div id="vtp" />

### Configuración del VTP

1. Entrar en modo de configuracion al switch

   ```
   > configure terminal
   ```

   

2. Configurar el switch en el modo vtp deseado

   ```
   > vtp domain [Nombre]		## asignar un nombre de dominio, este nombre debe ser igual en todos los switches
   > vtp password [Key]		## asignar la contraseña que se le dará al dominio, está debe ser igual en todos los switches
   > vtp version 2
   
   ------------- Para modo servidor
   > vtp mode server
   > vtp pruning
   
   ------------- Para modo cliente
   > vtp mode client
   ```

<div id="stp" />


### Configuración de STP

Esto se debe hacer unicamente en el switch que se configuró en modo *Server*

1. Entrar en modo de configuracion al switch

   ```
   > configure terminal
   ```

   

2. Activar el modo STP para el switch

   ```
   > spanning-tree vlan [n] root primary	## asignamos el número de vlan que deseamos que sea la principal
   ```



Para verificar que se ha creado correctamente la configuracion del STP se ejecutan los comandos siguientes:

```
> sh spanning-tree root			## muestra informacion de la prioridad de vlan's

> sh spanning-tree blockedport	## muestra los puertos que se han bloqueado por motivos de que esten causando algún loop

> sh spanning-tree brief 		## muestra toda la información proporcianda por el STP

```

