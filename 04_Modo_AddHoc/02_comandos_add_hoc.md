# Comandos Add Hoc

Ansible tiene la ventaja de ejecutar módulos de manera individual. Aunque no es el objetivo principal, ya que ansible esta definido para automatizar tareas, siempre es una ventaja poder ejecutar módulos de manera individual.

### Definición
-----

Un comando *add hoc*  es una orden que ejecuta de manera rápida una instrucción contral las máquinas especificadas. 

Aunque tiene mucha utilidad poder ejecutar ordenes de esta manera, tiene problema y es que no son reutilizables, es decir, no se pueden automatizar. 

Se utilizan, normalmente, para realizar pruebas. 

### Módulo *command*
-----

Este módulo nos permite enviar un comando contra una máquina remota. 

Tiene un pequeño inconveniente, y es que este módulo no se ejecuta sobre una shell de linux, lo que significa que no puedo ejecutar comandos avanzados de linux, como pueden ser filtros, tuberias, etc. 

- La sintaxis de este comando es la que sigue:

    `ansible -i maquinas debian1 -m command -a "date"`

    > Análizamos los *nuevos parámetros* de este comando: 
    - **"command"** -> Parámetro con el que indicamos cual vamos a poder utilizar ordenes de cli.
    - **"-a"** -> "arguments". Flag que nos permite utilizar argumentos. En este caso nuestro argumeto es el comando que queremos lanzar contra la máquina remota. 

- Más ejemplo: 

    `ansible -i maquinas debian1 -m command -a "ls -la /"`

    `ansible -i maquinas debian1 -m command -a "touch /tmp/f1.txt"`

> El comando **touch** de *linux*, es un comando que te crea un fichero si no existe o te modifica la fecha de modificación del fichero si existe. 

### Módulo *shell*
-----

Al contrario que *command*, **shell** si tiene esta función.  

- La sintaxis de este comando es la que sigue:

    `ansible -i maquinas debian1 -m shell -a "touch /tmp/f1.txt | grep p"`

> Vemos que la sintaxis es muy parecida a la de comando **command**

### Módulo *setup*
-----

Este módulo nos permite recuperar/obtener propiedades y carácteristicas de las máquinas remotas

- La sintaxis es la que sigue:

    `ansible -i maquinas tomcat1 -m setup`

> Este módulo nos devuelve un *json* con bastante información de la máquina remota a la cual hemos enviado la orden. 

### Módulo *copy*
-----

Este módulo nos permite realizar copia de ficheros a las máquinas remotas de manera sencilla. 

- La sintaxis es la siguiente:

    `ansible -i maquinas debian1 -m copy -a "src=./prueba.txt dest=/tmp/prueba.txt"`

Este es un ejemplo intuitivo de copia de un fichero llamado "prueba.txt"

#### Copia de un directorio

- Copia de directorios:

    `ansible -i maquinas debian 1 -m copy -a "src=dir1 dest="/tmp"`

Si ahora nos conectasemos a la máquina *debian1* por ssh y nos dirigieramos al directorio "/tmp", tendramos que ver tanto el fichero *prueba.txt* como el directorio *dir1*

#### Atributo *modo*, es decir, permisos

- Configuración de permisos (Lectura, escritura, ejecución):

    `ansible -i maquina debian1 -m copy -a "src=prueba.txt dest=/tmp/prueba1.txt mode=777"`

##### Hay una gran lista de módulos de ansible disponibles en su documentación . Por ejemplo, aquí tenemos la documentación del modulo **apt**. [Doc APT módule](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/apt_module.html)







