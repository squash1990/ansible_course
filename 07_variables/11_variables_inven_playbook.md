# Variables de inventarios en playbooks

-----
- Tags: #ansible #variables_inventarios #variables_task 
-----

### Uso y ejemplos
-----

- Creamos el playbook con el que vamos a realizar las pruebas:

	```yaml
	---
	- name: Prueba con variables dentro de "task"
	  hosts: debian1
	
	  tasks: 
	  - name: Variables local en la tarea.
	    vars:
	      v1: "Prueba de variable local"
	    debug:
	      msg: "Este es una forma de mostrar la {{ v1 }}"
	
	  - name: Us  o de la variable del inventario.
	    debug:
	      msg: "Este es una forma de mostrar la {{ v2 }}"
	...
	```

	> En este caso dará error, ya que no existe la variable v2. ¿Pero que pasa si la definimos en el inventario?

- Definimos la variable en nuestro inventario de tipo **yaml**

	```yaml
	all:
	  hosts:
		tomcat2:
	  children:
		debian:
		  hosts:
			debian1:
			  v2: "variable de inventario"
			debian2:
		rocky:
		  hosts:
			rocky1:
			rocky2:
		servidores_datos:
		  hosts:
			mysql1:
		servidores_web:
		  hosts:
			tomcat1:
	```

	> Al definir la variable en el inventario el playbook la buscara primero de manera local, después de manera global y por último de manera externa, es decir, va de abajo hacia arriba. 


**END**