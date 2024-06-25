# Creación de un usuario ansible con ssh configurado

En esta sección vamos a configurar el acceso ssh para un usuario no **root** 

### Creación de usuario en el nodo manejado
-----

- Para crear un usuario en el nodo manejado usamos

    `sudo useradd ansible`

    `sudo passwd ansible`

Normalmente en la mayoria de las distribuciones linux se creará un directorio de usuario en **/home/ansible**

### Configuración de ssh para el usuario ansible en el nodo de control
-----

- En primer lugar crearemos el usuario **ansible** en el nodo de control de la misma manera que lo hemos hecho en el nodo manejado para operar con el.

- A continuación nos logueamos con dicho usuario y creamos el par de claves para este usuario mediante la orden siguiente

    `ssh-keygen`

De esa manera ya tendriamos creadas las claves para el usuario **ansible** y se encontrarian en el directorio **/home/ansible/.ssh**

- Seguidamente copiamos la clave pública en el nodo manejado

    `ssh-copy-id ip_máquina_remota`

    > Nota: Si no especificamos el usuario, se va a realizar la copia utilizando el usuario con el cual ejecutamos la orden

El siguiene paso es configurar el comando **sudo** para que el usuario **ansible**, en el nodo de control, pueda ejecutar comandos como si fuera **root**. 

- Nos dirigimos al directorio **/etc/** y editamos **(como usuario root)** el fichero **sudoers**:
    `vi sudoers` 

    Dentro del fichero nos dirigimos al final del mismo y buscamos una linea que tenga el siguiente testo:

    `root = ALL(ALL:ALL) ALL`

    Copiamos esta linea y sustituimos **root** por **ansible**

    > De esta manera el usuario **ansible** puede ejecutar comandos como si fuera **root**

- También podremos añadirlo directamente al grupo sudo y para ello deberemos: 

    ```bash
	su -
	# Si estamos en based-distros debian
	usermod -aG sudo nombre_usuario 
	
	# Si estamos en based-distros redHat
	usermod -aG wheel nombre_usuario 
    ```


**END**





