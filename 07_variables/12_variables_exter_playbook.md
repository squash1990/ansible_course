# Variables externas de ficheros en playbooks

-----
- Tags: #ansible #variables_externas #variables_task #variables_de_ficheros
-----

### Uso y ejemplos
-----

En este caso, el propósito es crear ficheros que contengan las variables que se van a utilizar en los playbooks de manera que que todo mas organizado.

##### En primer lugar creamos los dos ficheros, llamados **fichero1.yaml** y **fichero2.yaml**

- "fichero1.yaml"

	```yaml
	f1: Este es el primer fichero
	```

- "fichero2.yaml"

	```yaml
	f2: Este es el segundo fichero
	```

##### Creamos el playbook 

- Playbook que hace uso de las variables de ficheros:

	```yaml
	---
	- name: Prueba con variables externas situadas en ficheros
	  hosts: debian1
	  vars_files:
	    - recursos/fichero1.yaml
	    - recursos/fichero2.yaml
	
	  tasks:
	  - name: Variable recuperada del primer fichero
	    debug:
	      msg: "Este es el valor del primer fichero: {{ f1 }}"
	
	  - name: Variable recuperada del segundo fichero
	    debug:
	      msg: "Este es el valor del segundo fichero: {{ f2 }}"
	...
	```

##### Vemos la salida de la ejecución de este playbook

- Ejecutamos el playbook:

	```bash
	$ ansible-playbook variables_de_ficheros.yaml 
	BECOME password: 
	
	PLAY [Prueba con variables externas situadas en ficheros] ********************************************************************************************
	
	TASK [Gathering Facts] *******************************************************************************************************************************
	[WARNING]: Platform linux on host debian1 is using the discovered Python interpreter at /usr/bin/python3.9, but future installation of another Python
	interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.18/reference_appendices/interpreter_discovery.html for
	more information.
	ok: [debian1]
	
	TASK [Variable recuperada del primer fichero] ********************************************************************************************************
	ok: [debian1] => {
	    "msg": "Este es el valor del primer fichero: Este es el primer fichero"
	}
	
	TASK [Variable recuperada del segundo fichero] *******************************************************************************************************
	ok: [debian1] => {
	    "msg": "Este es el valor del segundo fichero: Este es el segundo fichero"
	}
	
	PLAY RECAP *******************************************************************************************************************************************
	debian1                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
	```

	> Como podemos observar, ansible ha buscado la variable en los ficheros de variables que se han definido al comienzo del playbook y ha puesto su valor donde correspondía. 


**END**