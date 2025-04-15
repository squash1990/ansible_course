# Configuración de SSH para usuario root

### Secuencia de comandos para la correspodiente configuración
-----

#### Configuración de ssh para root en el nodo de control

- En primer lugar verificamos que tengamos instalado ssh:

    `which ssh`

- A continuación creamos el par de claves: 

    `ssh-keygen`

> Una vez ejecutado el anterior comando se nos habrán creado dos ficheros en el directorio **/home/squash/.ssh**. Estos dos ficheros son:
>
>     `id.rsa` -> Esta es nuestra clave privada
>
>     `id.rsa.pub` -> Esta es nuestra clave publica


- También podemos usar otros algoritmos como el que se indica: 

	```bash
	# Con la opción -t podemos indicar el tipo de algoritmo que en este caso es otro al popular "rsa"
	ssh-keygen -t ed25519 -C "squashwereables@gmail"
	```

- Podemos cargar la clave en el agente *ssh-agent* para que nos pida credenciales ni confirmación

	```bash
	# Proceso de carga del agente SSH
	eval "$(ssh-agent -s)"
	Agent pid 179426

	# Cargamos la clave SSH "PRIVADA"
	ssh-add ~/.ssh/id_ed25519
	Identity added: /home/squash/.ssh/id_ed25519 (squashwereables@gmail.com)
	```


#### Copia de la clave pública al nodo manejado y configuración del mismo.

En primer lugar verificamos que el servicio ***openSSH*** esté instalado, si no, lo instalamos:

- Instalamos **ssh**

	```bash
	sudo apt install openssh # Si estamos en debian
	dnf install openssh-server # Si estamos en redhat
	```

- Arrancamos el servicio:

	```bash
	systemctl start sshd # Arrancamos el servicio
	systemctl enable ssh # Habilitamos el arranque del servicio en el inicio del sistema
	```

Estando en el nodo manejado. Verificamos que tenemos disponible el directorio **/home/squash/.ssh**. 

- En el caso de que no lo tengamos creado lo creamos con

    `mkdir .ssh`

Ahora podemos copiar las claves directamente visualizando el fichero y copiandola o utilizando un comando especifico de ssh

- Copia de claves mediante ssh desde el nodo de control al nodo manejado:

    `ssh-copy-id -i ~/.ssh/id_rsa.pub "remote_ip"`

En este momento se habrá creado un fichero **authorized_keys** en la máquina remota donde se habrá añadido nuestra clave pública. 

- Permitir solo las claves ssh de los usuarios en los nodos manejados y que no nos permita entrar con **root**. Para ello, en el fichero de configuración **/etc/ssh/sshd_config** e incluimos las siguientes lineas

	```
	PasswordAuthentication no
	PermitRootLogin no
	```

- Volvemos a reiniciar el servicio ssh

- Por último intentamos conectarnos a los nodos por **ssh** desde el nodo de control al nodo manejado

	```bash
	ssh -l squash -i ~/.ssh/clave_publica.pub nombre_servidor
	```

#### Configuración del fichero *hosts* en el nodo de control 

Para poder usar directamente el nombre del nodo y no su ip añadimos las siguientes líneas al fichero ***/etc/hosts*** 

	```bash
	cat -np /etc/hosts
	# Host addresses
	127.0.0.1  localhost
	127.0.1.1  parrot
	172.17.0.2 ubuntu1
	172.17.0.3 ubuntu2
	172.17.0.4 ubuntu3
	172.17.0.5 ubuntu4
	::1        localhost ip6-localhost ip6-loopback
	ff02::1    ip6-allnodes
	ff02::2    ip6-allrouters
	# Others
	```

**END**