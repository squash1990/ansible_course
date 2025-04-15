# Ejercicios sobre ficheros de configuración de Ansible

En este apartado vamos a realizar algunos ejemplos prácticos para dejar asentado el uso y configuración de los ficheros de ansible. 

### Ejemplos
-----

- Preparar un fichero de configuración con varias opciones:
	- Creamos un directorio.
	- Copiamos el fichero de inventario que tengamos creado durante el curso.
	- Detro creamos un fichero denominado "ansible.cfg".
	- Ponemos los siguientes datos:

		```
		[defaults]
		inventory=./maquinas
		```

- Probamos lanzando un ping  para todas las máquinas.
- Ansible debe haber seleccionado el fichero de inventario correspondiente.
- Ahora añadimos al fichero de configuración lo siguiente:
	- Con esto lo que le estamos diciendo es que se convierta en el usuario *pepe* al ejecutar el proceso: 

		```
		[privelege_escalation]
		become = True
		become_user = pepe
		```

- Por último, ejecutamos de nuevo el comando ping. Veremos que su ejecución falla, ya que no existe dicho usuario. 


**END**