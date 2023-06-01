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

## Habilitar el envío de alertas para el autoticketing


## Agregar servidores al template Zabbix DB2

## Contribuciones
El equipo de Kyndryl Brasil que desarrolló el template Zabbix DB2:

Author: Rosimar Lima (rosimarl@kyndryl.com)

Contibutors:
  - Vinícius Freitas
  - Jose Henrique
  - Alexei Miranda 
  - David Fachini
  - Junior Silva

