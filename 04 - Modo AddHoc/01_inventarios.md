# Inventarios

En esta sección vamos a realizar un primer ejemplo de inventario.

### Definición
-----

Un inventario, es un fichero que contiene las máquinas con las que queremos trabajar, es decir, aquellas máquinas que van a ser manejadas por el nodo de control. Estas pueden estar presente dentro de dicho inventario, mediante su dirección Ip o mediante su nombre.

### Inventarios "maquinas"
-----

En primer lugar nos creamos un fichero llamado máquinas donde vamos a incorporar núestras máquinas. 

- Creamos el fichero **maquinas** e incorporamos máquinas según su IP

    ```bash
    vi maquinas

    # Contenido del fichero
    172.18.0.2
    ```

- Añadimos mas máquinas al *inventario* **maquinas** pero esta vez utilizando su FQDN

    ```bash
    debian1
    debian2
    rocky1
    rocky2
    ubuntu1
    mysql1
    tomcat1
    tomcat2
    ```

> Podremos usar la Ip o el nombre de la máquina si esta registrada en nuestro archivo **/etc/hosts** o en algún servidor de Dominio **dns**

### Prueba del inventario (Conectividad con las máquinas)
-----

#### Ejecución de módulos sobre todas las máquinas

Vamos a realizar la primera ejecución de comandos de ansible en modo Ad-Hoc, es decir, ejecución en línea de comandos.

- **(ansible -i inventario all -m módulo)**.Uso de ansible indicando el **inventario**

    `ansible -i maquinas all -m ping`

    > Análizamos este comando: 
    - **-i** -> Flag con el que indicamos cual es el inventario de máquinas a utilizar. A continuación, de este comando *indicamos el inventario*
    - **maquinas** -> Inventario que queremos utilizar
    - **all** -> Flag con el que indicamos a las máquinas que queremos que se vean afectadas por la orden
    - **m** -> Flag que habilita el uso de módulos. El nombre del módulo va a continuación.

> Nota: Este comando va a realizar un ping a todas las máquinas del inventario *maquinas*


#### Ejecución de módulos sobre las máquinas que se especifiquen

- Ejecutamos el módulo ping solo sobre la máquina *tomcat1*

    `ansible -i maquinas tomcat1 -m ping`

> De esta manera solo realizariamos el *ping* sobre la máquina *tomcat1*

- Uso de metacarácteres a la hora de indicar las máquinas sobre las cuales se quiere aplicar el módulo 

    `ansible -i maquinas tomcat* -m ping`


**END**