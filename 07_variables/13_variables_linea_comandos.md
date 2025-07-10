# Variables desde linea de comandos

-----
- Tags: #ansible #variables_externas #variables_task #variables_desde_linea_comandos #extra-vars
-----

### Uso y ejemplos: 

En este caso, el objetivo es introducir variables pero haciéndolo desde la misma linea desde la cual ejecutamos el *playbook*

- Creamos el playbook:

	```yaml
	---
	- name: Prueba con variables pasadas en linea de comandos
	  hosts: debian1
	
	  tasks:
	  - name: Variables desde linea de comandos
	    debug:
	      msg: "Pasamos el siguiente mensaje contenido en una variable introducida desde CLI. Mensaje: {{ mensaje }}"
	...
	```

- Ejecutamos el playbook, haciendo uso de la opcion **--extra-vars** para indicar la variable:

	```bash
	$ ansible-playbook variables_command_line.yaml --extra-vars "mensaje='Esto es una prueba de uso de variables en linea de comandos'"
	BECOME password: 
	
	PLAY [Prueba con variables pasadas en linea de comandos] *********************************************************************************************
	
	TASK [Gathering Facts] *******************************************************************************************************************************
	[WARNING]: Platform linux on host debian1 is using the discovered Python interpreter at /usr/bin/python3.9, but future installation of another Python
	interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.18/reference_appendices/interpreter_discovery.html for
	more information.
	ok: [debian1]
	
	TASK [Variables desde linea de comandos] *************************************************************************************************************
	ok: [debian1] => {
	    "msg": "Pasamos el siguiente mensaje contenido en una variable introducida desde CLI. Mensaje: Esto es una prueba de uso de variables en linea de comandos"
	}
	
	PLAY RECAP *******************************************************************************************************************************************
	debian1                    : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
	```

	> **Nota:** Hay que tener en cuenta que si el contenido de la variable *mensaje* tiene mas de una palabra, hay que ponerlo entre comillas simples y luego cerrar con comillas dobles.

- Comando de ansible con el uso de **--extra-vars**

	```bash
	$ ansible-playbook variables_command_line.yaml --extra-vars "mensaje='Esto es una prueba de uso de variables en linea de comandos'"
	```


**END**