# Variables FACT en playbooks

-----
- Tags: #ansible #variables_task #variables_fact #arrays #diccionarios 
-----

### Uso y ejemplos

El propósito de este punto es introducir el concepto de variables **fact**, ver su uso y aplicación. 

Las variables *fact* son variables que nos devuelven información y propiedades de las máquinas destino con las que estamos trabajando. 

Anteriormente vimos que para poder obtener este tipo de información teníamos que ejecutar el siguiente comando:

	ansible <máquina_objetivo> -m setup

> El módulo **setup** nos devuelve este tipo de información.

A continuación, vamos a ejecutar el anterior comando, pero vamos a guardar la información en un fichero de tipo *json*

	ansible debian1 -m setup > fact.json

Este fichero contiene toda la información relativa a esta máquina, es decir, todas las variables fact e información que nos puede ser muy útil, pero... ¿como puedo utilizar o invocar estas propiedades dentro de un playbook?

##### Creamos el playbook y vemos su salida.

- En primer lugar creamos el playbook donde vamos a utilizar la variable **fact** que queremos utilizar. 

	```yaml
	---
	- name: Uso de variables FACT en Playbooks
	  hosts: debian
	
	  tasks:
	  - name: Uso y aplicación de variables FACT
	    debug:
	      msg: "Arquitectura: {{ansible_facts['architecture']}} 
	            Bios vendor: {{ansible_facts['bios_vendor']}}"
	...
	```

- Ejecutamos el playbook:

	```bash
	$ ansible-playbook variables_fact.yaml 
	BECOME password: 
	
	PLAY [Uso de variables FACT en Playbooks] ************************************************************************************************************
	
	TASK [Gathering Facts] *******************************************************************************************************************************
	[WARNING]: Platform linux on host debian2 is using the discovered Python interpreter at /usr/bin/python3.9, but future installation of another Python
	interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.18/reference_appendices/interpreter_discovery.html for
	more information.
	ok: [debian2]
	[WARNING]: Platform linux on host debian1 is using the discovered Python interpreter at /usr/bin/python3.9, but future installation of another Python
	interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.18/reference_appendices/interpreter_discovery.html for
	more information.
	ok: [debian1]
	
	TASK [Uso y aplicación de variables FACT] ************************************************************************************************************
	ok: [debian1] => {
	    "msg": "Arquitectura: x86_64; Bios vendor: Phoenix Technologies LTD"
	}
	ok: [debian2] => {
	    "msg": "Arquitectura: x86_64; Bios vendor: Phoenix Technologies LTD"
	}
	
	PLAY RECAP *******************************************************************************************************************************************
	debian1                    : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	debian2                    : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
	```

##### Sacar información de variables fact que están en **arrays**

- Modificamos el playbook anterior para solicitar la variable que forma parte de un array:

	```yaml
	---
	- name: Uso de variables FACT en Playbooks
	  hosts: debian
	
	  tasks:
	  - name: Uso y aplicación de variables FACT
	    debug:
	      msg: "Arquitectura: {{ansible_facts['architecture']}};
	            Bios vendor: {{ansible_facts['bios_vendor']}}; 
	            IP: {{ansible_facts['all_ipv4_addresses'][0]}}"
	...
	```

	> Como podemos ver para sacar el valor de una variable de un array primero indicamos el nombre de la variable y luego la posición que se quiere sacar. 


##### Obtener la información de variables fact que son **diccionarios**

- Modificamos el playbook anterior para realizar esta tarea:

	```yaml
	---
	- name: Uso de variables FACT en Playbooks
	  hosts: debian
	
	  tasks:
	  - name: Uso y aplicación de variables FACT
	    debug:
	      msg: "Arquitectura: {{ansible_facts['architecture']}};
	            Bios vendor: {{ansible_facts['bios_vendor']}}; 
	            IP: {{ansible_facts['all_ipv4_addresses'][0]}}"
	
	  - name: Uso de diccionarios FACT
	    debug:
	      var: ansible_facts.cmdline.BOOT_IMAGE
	...
	```

	> Como podemos observar para visualizar el valor de una variable **fact** de tipo diccionario tenemos que usar el parámetro *"var:"* y a continuación **ansible_facts.<nombre_variable>.'clave'**.

- Ejecutamos el playbook:

	```bash
	$ ansible-playbook variables_fact.yaml 
	BECOME password: 
	
	PLAY [Uso de variables FACT en Playbooks] ************************************************************************************************************
	
	TASK [Gathering Facts] *******************************************************************************************************************************
	[WARNING]: Platform linux on host debian2 is using the discovered Python interpreter at /usr/bin/python3.9, but future installation of another Python
	interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.18/reference_appendices/interpreter_discovery.html for
	more information.
	ok: [debian2]
	[WARNING]: Platform linux on host debian1 is using the discovered Python interpreter at /usr/bin/python3.9, but future installation of another Python
	interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.18/reference_appendices/interpreter_discovery.html for
	more information.
	ok: [debian1]
	
	TASK [Uso y aplicación de variables FACT] ************************************************************************************************************
	ok: [debian1] => {
	    "msg": "Arquitectura: x86_64; Bios vendor: Phoenix Technologies LTD; IP: 172.18.0.2"
	}
	ok: [debian2] => {
	    "msg": "Arquitectura: x86_64; Bios vendor: Phoenix Technologies LTD; IP: 172.18.0.3"
	}
	
	TASK [Uso de diccionarios FACT] **********************************************************************************************************************
	ok: [debian1] => {
	    "ansible_facts.cmdline.BOOT_IMAGE": "/vmlinuz-6.4.0-150600.23.53-default"
	}
	ok: [debian2] => {
	    "ansible_facts.cmdline.BOOT_IMAGE": "/vmlinuz-6.4.0-150600.23.53-default"
	}
	
	PLAY RECAP *******************************************************************************************************************************************
	debian1                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	debian2                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
	```

##### Desactivar la recopilación de variables **FACTS**

Para ello, usamos el siguiente flag dentro del playbook:

	"gather_facts=no"


**END**