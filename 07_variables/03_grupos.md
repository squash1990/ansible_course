# Variables en inventarios. Grupos

Esta sección tiene como objetivo entender como realizar grupos de variables en los ficheros de inventario que afecten a un grupo de máquinas. 

-----
- Tags: #ansible #inventarios #variables #grupos 
-----

### Grupos de variables en ficheros de inventario
-----

#### Ficheros de tipo INI

Para declarar grupos de variables en ficheros de inventario se define el grupo de maquinas a las que se quiere que afecten la variable entre corchetes seguido de dos puntos y la variable, tal y como se muestra en el siguiente ejemplo:

- Ejemplo:

	```
	[debian]
	debian1
	debian2
	
	[debian:vars] # Grupo de variables que afectan al grupo de hosts "debian"
	puerto=9090
	ansible_user=admin
	
	[rocky]
	rocky1
	rocky2
	
	[ubuntu]
	ubuntu1 puerto=9090 entorno=desarrollo ansible_user=pepe
	
	[servidores_datos]
	mysql1
	mysql2
	
	[servidores_web]
	tomcat1
	tomcat2
	```

	> Como se puede observar en el código del inventario anterior, se ha definido un grupo de variables para el grupo de host debian. En este grupo hemos definido dos variables, una el puerto y otra variable especial **ansible_user=pepe**. Si lanzamos un comando, al no tener el usuario pepe puesto en el contenedor, nos tiene que dar error, vamos a verlo:
 
- Uso del inventario anterior:

	```bash
	ansible debian -m ping
	debian1 | UNREACHABLE! => {
	    "changed": false,
	    "msg": "Failed to connect to the host via ssh: pepe@debian1: Permission denied (publickey,password).",
	    "unreachable": true
	}
	debian2 | UNREACHABLE! => {
	    "changed": false,
	    "msg": "Failed to connect to the host via ssh: pepe@debian2: Permission denied (publickey,password).",
	    "unreachable": true
	}
	```

##### Variables de grupos anidados
-----

- En primer lugar vamos a definir los grupos anidados.
- En segundo lugar definimos las variables que afectan a ese grupo anidado, como se observa en la parte inferior del inventario que se muestra a continuación:

	```
	[debian]
	debian1
	debian2
	
	[debian:vars] # Grupo de variables que afectan al grupo de hosts "debian"
	puerto=9090
	ansible_user=admin
	
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
	tomcat2
	
	[desarrollo:children] # Definición de grupos anidados
	debian
	ubuntu
	
	[desarrollo:vars] # Definición de variables especiales que afectan al grupo anidado.
	ansible_user=squash
	```

	> Dejar claro que si se definen variables dentro de un grupo de host normal a nivel local, van a prevalecer por delante de las variables que se hayan definido dentro del grupo anidado.

#### Ficheros de tipo YAML

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
	  vars:
	    ansible_use: squash # aquí pondriamos las variables
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
		tomcat2:
```


**END**