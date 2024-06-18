# Comandos Add Hoc

Ansible tiene la ventaja de ejecutar comandos de manera individual. Aunque no es el objetivo principal, ya que ansible esta definido para automatizar tareas, siempre es una ventaja poder ejecutar comandos de manera individual.

### Definición
-----

Un comando *add hoc*  es una orden que ejecuta de manera rápida una instrucción contral las máquinas especificadas. 

Aunque tiene mucha utilidad poder ejecutar ordenes de estamanera, tiene problema y es que no son reutilizables, es decir, no se pueden automatizar. 

Se utilizan, normalmente, para realizar pruebas. 

#### Comando *command*
-----

Este parámetro nos permite enviar un comando contra una máquina remota. 

Tiene un pequeño inconveniente, y es que este comando no se ejecuta sobre una shell de linux, lo que significa que no puedo ejecutar comandos avanzados de linux, como pueden ser filtros, tuberias, etc. 

- La sintaxis de este comando es la que sigue:

    `ansible -i maquinas debian1 -m command -a "date"`

    > Análizamos los *nuevos parámetros* de este comando: 
    - **"command"** -> Parámetro con el que indicamos cual vamos a poder utilizar ordenes de cli.
    - **"-a"** -> "arguments". Flag que nos permite utilizar argumentos. En este caso nuestro argumeto es el comando que queremos lanzar contra la máquina remota. 

- Más ejemplo: 

    `ansible -i maquinas debian1 -m command -a "ls -la /"`

    `ansible -i maquinas debian1 -m command -a "touch /tmp/f1.txt"`

> El comando **touch** de *linux*, es un comando que te crea un fichero si no existe o te modifica la fecha de modificación del fichero si existe. 

### Comando *shell*
-----

Al contrario que *command*, **shell** si tiene esta función.  

- La sintaxis de este comando es la que sigue:

    `ansible -i maquinas debian1 -m shell -a "touch /tmp/f1.txt | grep p"`

> Vemos que la sintaxis es muy parecida a la de comando **command**







