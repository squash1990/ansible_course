# Escalar privilegios en Docker para la ejecución de comandos

Atendiendo a este apartado, con escalada de privilegios nos referimos a la situación en la que queremos ejecutar comandos ansible utilizando un usuario con el cual no estoy logueado. 

Por ejemplo, si queremos realizar una instalación de un paquete del sistema opeartivo, nuestro usuario no lo puede hacer, a no ser que, hayamos preparado dicho usuario dentro de la máquina remota para que pueda ejecutar **sudo** o conozcamos la *password de root* 

### Método de utilización
-----

Para realizar esta tarea utilizamos la cláusula ***become*** que nos permite convertirnos temporalmente en otro usuario distinto al que nos hemos conectado.

Internamente esta utilidad utiliza comandos habituales cómo **sudo**, **su**, **pbrun**, **runas**, etc.

#### Directivas de *become*

Este método utiliza una seríe de directivas que hay que especificar para realizar correctamente la escalada

- **become** -> Activa la escalada de privilegios.
- **become_user** -> Usuario en el cual queremos convertirnos.
- **become_method** -> El método que queremos utilizar dentro del sistema operativo remoto, lo más común es *sudo*.
- **become_flags** -> Permite especificar diferentes opciones en el momento de la escalada. 

### Ejemplos de utilización de directivas para escalar privilegios
-----

A continuación vamos a ver varios ejemplos, en los que nos vamos a convertir de un usuario a otro para utiliar un privilegio mayor. 

- En el caso de verificar la conectividad con una máquina remota mediante el uso del módulo **ping** no tendríamos que realizar una escalada de privilegios, solo bastaría con ejecutar el siguiente comando:

    `ansible -i maquinas ubuntu1 -m ping`

- Pero si queremos realizar la instalación de apache:

    `ansible -i maquinas ubuntu1 -m apt -a "name=apache2 state=present"`

    > Este comando ansible, tal cual, nos devolver un error de permisos, ya que no somos el usuario administrador y no podemos realizar instalaciones. 

- Utilizamos el flag ***"--become"*** para convertirnos en usuario **root**:

    `ansible -i maquinas ubuntu1 --become -m apt -a "name=apache2 state=present"`

    > Dependiendo de como está configurado este usuario en la máquina remota, nos dará un error de que se necesita contraseña para escalar privilegios

- Indicamos la *password* en la línea de ejecución:

    `ansible -i maquinas ubuntu1 --become --ask-become-pass -m apt -a "name=apache2 state=present"`

Finalmente vemos que el comando *apt* se ejecuta con normalidad para instalar el paquete **apache2**

### Ejemplos de escalar privilegios sin que nos pida contraseña
-----
 
Una de las formas para realizar este ejemplo sería realiar los pasos que se indican en el apartado -> [Acceso sin Password](../02_Entornos_gestionados/03_acceso_sin_password.md). 
Aunque no es optimo, ya que es una configuarción que a nivel de seguridad, es incorrecta. 

#### Escalada de privilegios utilizando [Acceso sin Password](../02_Entornos_gestionados/03_acceso_sin_password.md). 

- **(-b)**. Para realizar un **become**. Ejecutamos el siguiente comando:

    `ansible -i maquinas debian1 -m service -a "name=apache2 state=stopped" -b`

    > Podemos ver que en este caso, al estar configurado el fichero */etc/sudoers* para que no nos pida contraseña al utilizar **sudo**, no nos da error.

