# Variables registradas en playbooks

-----
- Tags: #ansible #variables_task #variables_registradas
-----

### Uso y ejemplos
-----

El propósito de este punto es introducir el concepto de variables **registradas**, ver su uso y aplicación.

Son un tipo de variables que permiten recuperar la salida de un comando que se esta ejecutando, guardarlo en dicha variable, para después poder utilizarlo.

##### Playbook

- En primer lugar creamos el playbook de ejemplo:

	```yaml
	---
	- name: Variables registradas
	  hosts: debian1
	
	  tasks:
	  - name: Capturar fecha
	    shell:
	      cmd: date
	...
	```

- Ejecutamos el playbook y observamos su salida:

	```bash
	$ ansible-playbook variables_registradas.yaml 
	BECOME password: 
	
	PLAY [Variables registradas] *************************************************************************************************************************
	
	TASK [Gathering Facts] *******************************************************************************************************************************
	[WARNING]: Platform linux on host debian1 is using the discovered Python interpreter at /usr/bin/python3.9, but future installation of another Python
	interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.18/reference_appendices/interpreter_discovery.html for
	more information.
	ok: [debian1]
	
	TASK [Capturar fecha] ********************************************************************************************************************************
	changed: [debian1]
	
	PLAY RECAP *******************************************************************************************************************************************
	debian1                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
	```

	> Como se puede observar, el playbook se ejecuta correctamente. Se indica que se han producido cambios de manera correcta pero ..., no nos devuelve la fecha. 

Para que el playbook nos devuelva la fecha, en este caso, necesitamos registrar la salida del comando en una variable.

- Se modifica el playbook anterior para registrar la variable:

	```yaml
	---
	- name: Variables registradas
	  hosts: debian1
	
	  tasks:
	  - name: Capturar fecha
	    shell:
	      cmd: date
	    register: fecha
	
	  - name: Mostrar fecha utilizando "msg"
	    debug:
	      msg: "{{ fecha }}"
	
	  - name: Mostrar fecha utilizando "var"
	    debug:
	      var: fecha
	...
	```

	> Como se puede observar, se han utilizado la opción **register** para almacenar la salida del comando "cmd" en la variable fecha. A continuación, se han creado dos tareas con el fin de ver dos maneras de visualizar el valor de esta variable, utilizando el modulo **msg** o **var**.

##### Visualización de variables de tipo diccionario en la salida de un comando de ansible

Ansible devuelve la información solicitada en formato **json**. Por tanto, si queremos acceder a ella debemos hacerlo dependiendo si es un array o un diccionario. 

- Modificamos el playbook anterior:

	```yaml
	--
	- name: Variables registradas
	  hosts: debian1
	
	  tasks:
	  - name: Capturar fecha
	    shell:
	      cmd: date
	    register: fecha
	
	  - name: Mostrar fecha utilizando "msg"
	    debug:
	      msg: "{{ fecha.stdout }}"
	
	  - name: Mostrar fecha utilizando "var"
	    debug:
	      var: fecha.stdout
	...
	```


**END**