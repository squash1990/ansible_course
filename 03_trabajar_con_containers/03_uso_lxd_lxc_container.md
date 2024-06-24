# Uso y configuración de contenedores LXC 

En esta sección vamos a introducir el uso de los contenedores linux 

----
- Tags: #ansible #lxd #containers 
-----

### Definición
-----

Este es uno de los software que podemos utilizar para crear y gestionar contenedores. Es software de Canonical 

### Uso
-----

- Para instalar este software ejecutamos el siguiente comando:
	```bash
	sudo apt install lxd 
	
	# En ubuntu:
	sudo snap install lxd
	```

- A continuación lo **inicializamos** con **lxc**:
	```bash
	lxd init
	```

- Para **listar** contenedores, que tengamos corriendo, usamos:
	```bash
	lxc list
	```

- Para **listar** imágenes remotas
	```bash
	lxc remote list
	```

- Para **listar** las imágenes del repositorio:
	```bash
	lxc image list images:
	```

- Arrancar un contenedor en base a una de las imagenes:
	```bash
	sudo lxc launch ubuntu:22.04 Ubuntu2204
	```

- Para arrancar o parar un contenedor
	```bash
	lxc start
	lxc stop
	```

	>Con este comando creamos un contendor partiendo de la imagen **ubuntu22.04** y lo creamos con el nombre **Ubuntu2204**

- Para conectarnos al contenedor ejecutamos el siguiente comando:
	```bash
	sudo lxc exec Ubuntu2204 -- /bin/bash
	```


**END**