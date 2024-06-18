# Módulos Yum, apt, service

Ansible dispone de módulos para la manipulación de paquetes de un sistema, es decir, instalar, buscar, eliminar paquetes

### Módulo *Yum*
-----

*ansible.builtin.yum module* -> Este paquete forma parte del core de Ansible

Este módulo nos permite gestionar los paquetes en distribuciónes basadas en *Red Hat*

#### Sintaxix y ejemplos

- La sintaxis de este comando es la siguiente:

    `ansible -i maquinas rocky1 -m yum -a "name=httpd state=present"`

    > Análizamos los *nuevos parámetros* de este comando: 
    - **"yum"** -> Módulo que utilizamos para gestionar los paquetes
    - **"-a"** -> "arguments". Flag que nos permite utilizar argumentos. En este caso nuestro argumeto es el comando que queremos lanzar contra la máquina remota. 
    - **"name"** -> Nombre del paquete con el que queremos operar.
    - **"state"** -> Estado final del paquete que queremos que tenga. 
        - Estos estados pueden ser: 
            - **"present"** -> Instalamos el paquete
            - **"absent"** -> Desinstalamos el paquete
            - **"removed"** -> Eliminamos el paquete
            - **"latest"** -> Instalamos la ultima versión


> Podemos profundizar mas en sus argumentos consultando su documentación -> [Ansible doc Yum](https://docs.ansible.com/ansible/9/collections/ansible/builtin/yum_module.html)