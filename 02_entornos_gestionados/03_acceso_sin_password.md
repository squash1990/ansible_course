# Configuración de acceso sin contraseña

En esta sección vamos a explicar como configurar el acceso por **ssh** sin que se nos solicite la contraseña.

### Edición del fichero **/etc/sudoers** 
-----

A continuación, vamos a proceder a editar el fichero sudoers para configurarlo de tal manera que no nos pida contraseña de usuario cuando utilizamos la orden **sudo** al ejecutar un comando como **root**

>   Este fichero lo podemos editar usando **vim** o **visudo**. Es mas conveniente realizarlo con visudo, ya que este comando se adapta a las reglas de sintaxis del fichero y en el caso de que configuremos algo que no cumpla dichas reglas no se permitirá su modificación. 

- Editamos el fichero **/etc/sudoers** y añadimos la linea como aparece a continuación 

    ```bash
    visudo /etc/sudoers

    # Al añadir "NOPASSWD" delante de "ALL" haremos que no se nos pida password
    ansible    ALL=(ALL) NOPASSWD:ALL 

    ```


**END**
     






