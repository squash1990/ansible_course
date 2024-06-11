# Configuración de SSH para usuario root

### Secuencia de comandos para la correspodiente configuración
-----

#### Configuración de ssh para root en el nodo de control

- En primer lugar verificamos que tengamos instalado ssh:

    `which ssh`

- A continuación creamos el par de claves: 

    `ssh-keygen`

Una vez ejecutado el anterior comando se nos habrán creado dos ficheros en el directorio **/home/squash/.ssh**. Estos dos ficheros son:

    `id.rsa` -> Esta es nuestra clave privada

    `id.rsa.pub` -> Esta es nuestra clave publica

#### Copia de la clave pública al nodo manejado

Estando en el nodo manejado. Verificamos que tenemos disponible el directorio **/home/squash/.ssh**. 

- En el caso de que no lo tengamos creado lo creamos con

    `mkdir .ssh`

Ahora podemos copiar las claves directamente visualizando el fichero y copiandola o utilizando un comando especifico de ssh

- Copia de claves mediante ssh

    `ssh-copy-id "remote_ip"`

En este momento se habrá creado un fichero **authorized_keys** en la máquina remota donde se habrá añadido nuestra clave pública. 

