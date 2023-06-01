# Template Zabbix para monitoreo DB2

## Objetivo

El objetivo es habilitar el monitoreo de los servidores con base de datos DB2.

## Pre-requisitos
* Agente Zabbix instalado en los servidores que serán monitoreados.
* Agente Zabbix debe ejecutar como root.
* El team de DM debe dar privilegios a las instancias y bases de datos involucradas en el monitoreo.

## Ejecutar agente Zabbix como root

### En Linux:
Para que el agente Zabbix ejecute como root:
Primero, se debe cambiar la configuración a nivel de systemctl.
Segundo, hacer los cambios en la configuración del /etc/zabbix/ zabbix_agentd.conf

**Ejecutar**: systemctl edit zabbix-agent

Se abrirá un editor en blanco, se debe agregar lo siguiente y luego guardar cambios con :wq!

    [Service]
    User=root
    Group=root

Para verificar el cambio:

**Ejecutar**: systemctl cat zabbix-agent 

**Ejecutar**: systemctl daemon-reload

Ahora, hacer la modificación en el archivo de configuración del agente Zabbix:

cd /etc/zabbix/
vi zabbix_agentd.conf

Agregar los siguientes 3 parámetros y grabar con :wq!

    User=root
    AllowRoot=1
    AllowKey=system.run[*]

Reiniciar el agente Zabbix

**Ejecutar**: systemctl restart zabbix-agent

Verificar agente ejecutando como root:

**Ejecutar**: ps -fea | grep zabbix_agent


### En AIX:

No hay systemctl, solo hay que agregar lo siguiente al archivo de configuración de zabbix:

/etc/zabbix/zabbix_agentd.conf

    User=root
    AllowRoot=1
    AllowKey=system.run[*]

Luego reiniciar el agente Zabbix


## Importar template Zabbix

Descargar el archivo "zbx_export_templates_DB2.yaml" al equipo local

Ingresar a la consola Zabbix:

Configuration / Templates

![](https://github.com/Efra28/CrearReadmes/blob/main/template_zabbix_db2/Screens/zabbix_1.jpg)

"Import"

![](https://github.com/Efra28/CrearReadmes/blob/main/template_zabbix_db2/Screens/zabbix_2.jpg)

Elegir "zbx_export_templates_DB2.yaml" y presionar "Import"

![](https://github.com/Efra28/CrearReadmes/blob/main/template_zabbix_db2/Screens/zabbix_3.jpg)

Volver a presiónar "Import"

Verificar que el template importado se visualice:

![](https://github.com/Efra28/CrearReadmes/blob/main/template_zabbix_db2/Screens/zabbix_4.jpg)

Para que las alertas puedan generar autotiketing de forma correcta, se debe editar todos los Triggers y Trigger Prototypes, de tal manera que inicie con: <código del cliente>_<código de alerta>. Por ejemplo:

![](https://github.com/Efra28/CrearReadmes/blob/main/template_zabbix_db2/Screens/zabbix_5.jpg)

Ahora se debe crear el template para el cliente, en base a ese template base Innovation DB2

"Create Template"

![](https://github.com/Efra28/CrearReadmes/blob/main/template_zabbix_db2/Screens/zabbix_6.jpg)

En la pestaña "Template" escribir el nombre del nuevo template DB2 para el cliente (En este caso el cliente es "Unique")

Agregar el grupo (Si no existe, crearlo previamente)

![](https://github.com/Efra28/CrearReadmes/blob/main/template_zabbix_db2/Screens/zabbix_7.jpg)

En la pestaña "Linked Templates" agregar el template base innovation DB2

![](https://github.com/Efra28/CrearReadmes/blob/main/template_zabbix_db2/Screens/zabbix_8.jpg)

Verificar que el nuevo template para el cliente esté creado

![](https://github.com/Efra28/CrearReadmes/blob/main/template_zabbix_db2/Screens/zabbix_9.jpg)

Dar click al nombre del nuevo template y crear la macro {$CUSTOMER.CODE} con el valor del código del cliente (en este caso "unq") y presionar el botón "Update"

![](https://github.com/Efra28/CrearReadmes/blob/main/template_zabbix_db2/Screens/zabbix_10.jpg)

## Habilitar el envío de alertas para el autoticketing

Configuration / Actions / Trigger Actions

![](https://github.com/Efra28/CrearReadmes/blob/main/template_zabbix_db2/Screens/zabbix_11.jpg)

Ubicar el Trigger Action relacionado al cliente y dar click al nombre

![](https://github.com/Efra28/CrearReadmes/blob/main/template_zabbix_db2/Screens/zabbix_12.jpg)

Presionar "Add" y seleccionar el template DB2 creado para el cliente. Botón "Select" y botón "Update"

![](https://github.com/Efra28/CrearReadmes/blob/main/template_zabbix_db2/Screens/zabbix_13.jpg)

## Agregar servidores al template Zabbix DB2

Configuration / Hosts

Buscar el nombre del host al que se le quiere vincular el template DB2

![](https://github.com/Efra28/CrearReadmes/blob/main/template_zabbix_db2/Screens/zabbix_14.jpg)

Click sobre el nombre del host

![](https://github.com/Efra28/CrearReadmes/blob/main/template_zabbix_db2/Screens/zabbix_15.jpg)

Agregar el grupo correspondiente

![](https://github.com/Efra28/CrearReadmes/blob/main/template_zabbix_db2/Screens/zabbix_16.jpg)

En la pestaña "Templates" agregar el template DB2 creado para el cliente. Botón "Select" y "Update"

![](https://github.com/Efra28/CrearReadmes/blob/main/template_zabbix_db2/Screens/zabbix_17.jpg)

## Contribuciones
El equipo de Kyndryl Brasil que desarrolló el template Zabbix DB2:

Author: Rosimar Lima (rosimarl@kyndryl.com)

Contibutors:
  - Vinícius Freitas
  - Jose Henrique
  - Alexei Miranda 
  - David Fachini
  - Junior Silva

