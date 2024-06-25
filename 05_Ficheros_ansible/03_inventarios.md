# Inventarios de Ansible

Análizamos los ficheros correspondientes a los invetarios de máquinas que tenemos en [Ansible](../00_Intro/00_Ansible.md).

-----
- Tags: #ansible #inventarios
-----

### Definición
-----

 Es un fichero donde quedan resgistrados todos los nodos manejados o equipos, servidores a los cuales nos queremos conectar y realizar tareas.

 Puede ser un único fichero, una lista o usar plugins más avanzados y dinámicos que nos permitan personalizar los datos

 Para construir este tipo de ficheros, se pueden usuar distingos formatos, como *YAML*, *INI*, etc. 

 - Por defecto, ansible busca el fichero úbicado en **/etc/ansible/hosts**, pero hay que dejar constancia que mediante el uso del flag **"-i"** podemos indicar otra ubicación 

    `ansible -i inventory_file`

### Estructura de los ficheros
-----

- Los **host** o equipos que queramos gestionar, los podemos introducir tanto por IP como por FQDN (Nombre de dominio)

- Los podemos definir en formato **.yaml** o en formato **INI**

#### ***Fichero INI***

- Su estructura es la que sigue:

    ```ini
	ubuntu1.local
	ubuntu2.local
	````

- Otro ejemplo:

    ```ini
	datos1

    [servidores_aplicaciones]
    tomcat1
    tomcat2

    [servidores_datos]
    datos1.empresa.com
    datos2.empresa.com
	````

#### ***Fichero YAML***

En primer lugar definimos lo que es **yaml**[^1]. YAML es el lenguaje de serialización de datos utilizado por [Ansible](../00_Intro/00_Ansible.md) para la definición de los ficheros de definición de tareas (playbooks).

- Sus caracteristicas principales son: 

	- Legible
	- Independiente del lenguaje
	- Unicode
	- Sirve para serializar datos de objetos
	- Se pruede leer de una sola vez
	- etc.

- Su sintaxis general:

	- Se basan en el uso de (**Clave: Valor**)
	- Sus ficheros **empiezan** por **---** y **terminan** por **...***
	- Los comentarios se hacen con **#**
	- Se basan en grados de identación. La identación se realiza mediante espacios en blanco.
	- Se pueden realizar objetos en modo arbol, anidado.

- Su estructura es la siguiente:

    ```yaml
	all:
	 hosts: # Máquinas no agrupadas
	  ubuntu1:
	   ansible_host: 172.17.0.2
	  ubuntu2:
	   ansible_host: 172.17.0.3
	  ubuntu3:
	   ansible_host: 172.17.0.4
	  ubuntu4:
	   ansible_host: 172.17.0.5
	 children:
	  ubuntu:
	   hosts:
	    ubuntu1:
	    ubuntu2:
	  webservers:
	   hosts:
	    ubuntu3:
	    ubuntu4:
	```

#### ***Grupos de máquinas***

> Como podemos ver se puden crear grupos de equipos o servidores para racionalizar el uso de despliegues.

- Hay al menos dos grupos predefinidos

    - **ALL**. Contiene todos los servidores dentro del inventario
    - **UNGROUPED**. Contiene los servidores que no se han asociado a ningún grupo

		`ansible ungrouped -m ping` 

    - Se pueden poner equipos en ambas partes y/o grupos. 

- Se pueden anidar los grupos

#### ***Rangos***

Se pueden indicar rangos de maquinas de la siguiente manera:

    ```ini
	datos1

    [servidores_aplicaciones]
    tomcat[1:20]

    [servidores_datos]
    datos[1:4]empresa.com
	```

### Uso de los ficheros 
-----

Para usar los fichero, ya hemos indicado que tenemos que usar las opción **-i**. También podemos indicar varios ficheros en la misma orden. 

- Uso de varios ficheros de inventarios:

    `ansible -i inventario1 -i inventario2`

- La ejecución de los inventarios se hace en orden por lo que hay que tener cuidado con esto.


### Sintaxis y comprobación
-----

- En primer lugar visualizamos un inventario de ejemplo:

	```bash
	cat -np inventory
	172.17.0.2
	172.17.0.3
	172.17.0.4
	172.17.0.5
	```

- A continuación podemos realizar un ping en ansible para comprobar que funciona correctamente:

	```bash
	ansible all -i inventory -m ping

	172.17.0.2 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
	}
	172.17.0.4 | SUCCESS => {
	    "ansible_facts": {
	        "discovered_interpreter_python": "/usr/bin/python3"
	    },
	    "changed": false,
	    "ping": "pong"
	}
	172.17.0.3 | SUCCESS => {
	    "ansible_facts": {
	        "discovered_interpreter_python": "/usr/bin/python3"
	    },
	    "changed": false,
	    "ping": "pong"
	}
	```

- Otra manera mas ordenada sería: 

	```bash
	ansible-inventory -i inventory --list
	{
	    "_meta": {
	        "hostvars": {}
	    },
	    "all": {
	        "children": [
	            "ungrouped"
	        ]
	    },
	    "ungrouped": {
	        "hosts": [
	            "172.17.0.2",
	            "172.17.0.3",
	            "172.17.0.4",
	            "172.17.0.5"
	        ]
	    }
	}
	```


- Ahora vamos a crear otro inventario pero con dns:
	
	```bash
	cat -np inventory-dns
	ubuntu1
	ubuntu2
	ubuntu3
	ubuntu4

	ansible-inventory -i inventory-dns --list
	{
	    "_meta": {
	        "hostvars": {}
	    },
	    "all": {
	        "children": [
	            "ungrouped"
	        ]
	    },
	    "ungrouped": {
	        "hosts": [
	            "ubuntu1",
	            "ubuntu2",
	            "ubuntu3",
	            "ubuntu4"
	        ]
	    }
	}
	```

### Referencias

[^1]: Pagina web de IAML -> [YAML](https://yaml.org)


**END**

