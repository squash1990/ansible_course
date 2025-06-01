# Variables en task

-----
- Tags: #ansible #variables #task #variables_task #variables_globales
-----

### Uso y ejemplos
-----

#### Prueba con variables locales

- En primer definimos un playbook sencillo para visualizar como se usan aquí las variables y después realizar pruebas. 

	```yaml
	- name: Prueba con variables dentro de "task"
	  hosts: debian1
	
	  tasks:
	  - name: Variables local en la tarea
	    vars:
	      v1: "Prueba de variable local"
	    debug:
	      msg: "Este es una forma de mostrar la {{ v1 }}"
	...
	```

- Ahora definimos otra tarea **taks** e intentamos usar la variable definida en la primera tarea en esta nueva. 

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
	
	  - name: Uso de la variable local anterior en esta tarea.
	    debug:
	      msg: "Este es una forma de mostrar la {{ v1 }}"
	...
	```

- Visualizamos la salida resultante de la ejecución del anterior playbook:

	```bash
	ansible-playbook variables_task.yaml 
	BECOME password: 
	
	PLAY [Prueba con variables dentro de "task"] *********************************************************************************************************
	
	TASK [Gathering Facts] *******************************************************************************************************************************
	[WARNING]: Platform linux on host debian1 is using the discovered Python interpreter at /usr/bin/python3.9, but future installation of another Python
	interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.18/reference_appendices/interpreter_discovery.html for
	more information.
	ok: [debian1]
	
	TASK [Variables local en la tarea.] ******************************************************************************************************************
	ok: [debian1] => {
	    "msg": "Este es una forma de mostrar la Prueba de variable local"
	}
	
	TASK [Uso de la variable local anterior en esta tarea.] **********************************************************************************************
	fatal: [debian1]: FAILED! => {"msg": "The task includes an option with an undefined variable.. 'v1' is undefined\n\nThe error appears to be in '/home/squash/Desktop/squash/ansible/practicas/variables_task/variables_task.yaml': line 12, column 5, but may\nbe elsewhere in the file depending on the exact syntax problem.\n\nThe offending line appears to be:\n\n\n  - name: Uso de la variable local anterior en esta tarea.\n    ^ here\n"}
	
	PLAY RECAP *******************************************************************************************************************************************
	debian1                    : ok=2    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
	```

	> Como podemos ver al utilizar la variable local definida en la primera tarea, en la segunda, da error, ya que, al ser local en la primera tarea no está definida en la segunda y no la encuentra. 

- Si definimos de nuevo la variable en la task 2 vemos que si se puede utilizar: 

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
	
	  - name: Uso de la variable local anterior en esta tarea.
	    vars:
	      v1: "Prueba con la variable local de la tarea 2"
	    debug:
	      msg: "Este es una forma de mostrar la {{ v1 }}"
	...
	```

- Visualizamos su salida:

	```bash
	ansible-playbook variables_task.yaml 
	BECOME password: 
	
	PLAY [Prueba con variables dentro de "task"] *********************************************************************************************************
	
	TASK [Gathering Facts] *******************************************************************************************************************************
	[WARNING]: Platform linux on host debian1 is using the discovered Python interpreter at /usr/bin/python3.9, but future installation of another Python
	interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.18/reference_appendices/interpreter_discovery.html for
	more information.
	ok: [debian1]
	
	TASK [Variables local en la tarea.] ******************************************************************************************************************
	ok: [debian1] => {
	    "msg": "Este es una forma de mostrar la Prueba de variable local"
	}
	
	TASK [Uso de la variable local anterior en esta tarea.] **********************************************************************************************
	ok: [debian1] => {
	    "msg": "Este es una forma de mostrar la Prueba con la variable local de la tarea 2"
	}
	
	PLAY RECAP *******************************************************************************************************************************************
	debian1                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
	```

#### Prueba con variables locales y globales

- Creamos un playbook para ver la funcionalidad del uso de variables locales y globales

	```yaml
	---
	- name: Prueba con variables dentro de "task" y globales
	  hosts: debian1
	  vars:
	    v1: "variables globales"
	
	  tasks:
	  - name: Variables local en la tarea.
	    vars:
	      v1: "Prueba de variable local"
	    debug:
	      msg: "Este es una forma de mostrar la {{ v1 }}"
	
	  - name: Uso de la variable local anterior en esta tarea.
	    vars:
	      v1: "Prueba con la variable local de la tarea 2"
	    debug:
	      msg: "Este es una forma de mostrar la {{ v1 }}"
	
	  - name: Uso de la variable global en esta tarea.
	    debug:
	      msg: "Este es una forma de mostrar la {{ v1 }}"
	...
	```

- Visualizamos la salida:

	```bash
	ansible-playbook variables_task.yaml 
	BECOME password: 
	
	PLAY [Prueba con variables dentro de "task" y globales] *********************************************************************************************************
	
	TASK [Gathering Facts] *******************************************************************************************************************************
	[WARNING]: Platform linux on host debian1 is using the discovered Python interpreter at /usr/bin/python3.9, but future installation of another Python
	interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.18/reference_appendices/interpreter_discovery.html for
	more information.
	ok: [debian1]
	
	TASK [Variables local en la tarea.] ******************************************************************************************************************
	ok: [debian1] => {
	    "msg": "Este es una forma de mostrar la Prueba de variable local"
	}
	
	TASK [Uso de la variable local anterior en esta tarea.] **********************************************************************************************
	ok: [debian1] => {
	    "msg": "Este es una forma de mostrar la Prueba con la variable local de la tarea 2"
	}
	
	TASK [Uso de la variable global en esta tarea.] ******************************************************************************************************
	ok: [debian1] => {
	    "msg": "Este es una forma de mostrar las variables globales"
	}
	
	PLAY RECAP *******************************************************************************************************************************************
	debian1                    : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
	```


**END**