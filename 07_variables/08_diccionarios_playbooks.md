# Uso de diccionarios en playbooks 

-----
- Tags: #ansible #diccionarios 
-----

### Definición
-----

Conjunto de propiedades en forma de clave - valor.

### Uso y ejemplos
-----

A continuación vamos a ver el uso de diccionarios

- Definimos el playbook en el cual vamos a usar el diccionario:

	```yaml
	---
	- name: Prueba con Diccionarios
	  hosts: debian1
	  vars:
	    mensaje: "Este es un playbook en el cual se muestra el uso de diccionarios"
	    curso: "Capítulo 7 del curso de ansible"
	    desarrollo:
	      tipo: linux
	      memoria: 4GB
	      disco: 500GB
	
	  tasks:
	  - name: Ver variables de tipo diccionario
	    debug:
	      msg: "El tipo de máquina virtual es: {{ desarrollo }}"
	...
	```

- La salida por pantalla resultante de la ejecución de este playbook es: 

	```bash
	$ ansible-playbook dic_playbook.yaml 
	
	PLAY [Prueba con Diccionarios] *****************************************************************************************************************************************
	
	TASK [Gathering Facts] *************************************************************************************************************************************************
	[WARNING]: Platform linux on host debian1 is using the discovered Python interpreter at /usr/bin/python3.9, but future installation of another Python interpreter could
	change the meaning of that path. See https://docs.ansible.com/ansible-core/2.18/reference_appendices/interpreter_discovery.html for more information.
	ok: [debian1]
	
	TASK [Ver variables de tipo diccionario] *******************************************************************************************************************************
	ok: [debian1] => {
	    "msg": "El tipo de máquina virtual es: {'tipo': 'linux', 'memoria': '4GB', 'disco': '500GB'}"
	}
	
	PLAY RECAP *************************************************************************************************************************************************************
	debian1                    : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
	```

- Si queremos acceder a algún elemento:

	```yaml
	---
	- name: Prueba con Diccionarios
	  hosts: debian1
	  vars:
	    mensaje: "Este es un playbook en el cual se muestra el uso de diccionarios"
	    curso: "Capítulo 7 del curso de ansible"
	    desarrollo:
	      tipo: linux
	      memoria: 4GB
	      disco: 500GB
	
	  tasks:
	  - name: Ver variables de tipo diccionario
	    debug:
	      msg: "El tipo de máquina virtual es: {{ desarrollo.tipo }}. Su memoria es de: {{ desarrollo.memoria }}"
	...
	```

	> Como podemos ver, para acceder a algún elemento en concreto su sintaxis es **{{ desarrollo.tipo }}**


**END**