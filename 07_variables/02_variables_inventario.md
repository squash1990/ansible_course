# Variables en ficheros de inventario

En esta sección vamos a ver como se definen y usan las variables en los ficheros de inventario de ansible. 

-----
- Tags: #ansible #variables #ficheros_de_inventario #INI #yaml #variables_especiales
-----

### Variables en ficheros de inventario
-----

#### [Inventarios](../05_ficheros_ansible/03_inventarios.md) de tipo INI 
-----

A continuación, vamos a ver como se declaran o definen variables dentro de un fichero de inventario de tipo *INI*

- Teniendo un inventario de tipo como el que hemos venido configurando:

	```
	[debian]
	debian1
	debian2
	
	[rocky]
	rocky1
	rocky2
	
	[ubuntu]
	ubuntu1 puerto=9090 entorno=desarrollo
	
	[servidores_datos]
	mysql1
	mysql2
	
	[servidores_web]
	tomcat1
	tomcat1
	```

	> Como podemos ver las varibales se definen, normalmente, detrás del nombre de la máquina y siguiendo un patron de **"nom_variable=valor_variable"**

Estas variables que han sido definidas en el fichero de inventario son pasadas al playbook con el cual se esta trabajando. 

#### [Inventarios](../05_ficheros_ansible/03_inventarios.md) de tipo YAML
-----

- A continuación, vamos a ver como se declaran o definen variables dentro de un fichero de inventario de tipo **YAML**

	```yaml
	all:
	  hosts:
		ubuntu1:
		  puerto: 9000
		  entorno: desarrollo
		debian1:
	
	  children:
		debian:
		  hosts:
			debian1:
			debian2:
		rocky:
		  hosts:
			rocky1:
			rocky2:
		servidores_datos:
		  hosts:
			mysql1:
			mysql2:
		servidores_web:
		  hosts:
			tomcat1:
	```

### Variables especiales.

Existe un tipo de variables especiales que nos permiten definir mejor el comportamiento a la hora de realizar ejecuciones. Algunas de estas variables son las siguientes, aunque existe muchas más y que podemos consultar en la documentación de [ansible](../00_intro/00_Ansible.md).

- **ansible_host**: permite cambiar el nombre de la máquina.
- **ansible_port**: puerto de conexión. Sabemos que ansible usa SSH.
- **ansible_user**: usuario utilizado para realizar las conexiones.
- **ansible_become**: variable utilizada para realizar escalada de privilegios.
- **ansible_become_user**: usuario en el que nos convertiremos al realizar la escalada.
- **ansibly_python_interpreter**: variable para indicar la ruta en la que se encuentra ansible.
- etc...

####  Ejemplo de uso de variables especiales en los inventarios de tipo INI:

	```
	[debian]
	debian1
	debian2
	
	[rocky]
	rocky1
	rocky2
	
	[ubuntu]
	ubuntu1 puerto=9090 entorno=desarrollo ANS ansible_user=pepe
	
	[servidores_datos]
	mysql1
	mysql2
	
	[servidores_web]
	tomcat1
	tomcat1
	```


**END**