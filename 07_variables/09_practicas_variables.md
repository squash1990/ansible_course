# Prácticas con variables

-----
- Tags: #ansible #variables #playbooks
-----

### Prácticas
-----

#### Modificar el fichero /etc/motd de linux

En primer lugar vamos a crear un playbook para modificar el fichero **motd** de linux que permite personalizar el mensaje de bienvenida al entrar en un sistema linux. 

- Creamos el playbook: 
	
	```yaml
	---
	- name: Prueba con variables para personalizar motd
	  hosts: debian1
	
	  tasks:
	  - name: Customizar motd, mensaje de bienvenida
	    copy:
	      content: "Hello squash. Here again? Do you know you are welcome? "
	      dest: /etc/motd
	...
	```

- Ejecutamos el playbook:

	```bash
	$ ansible-playbook customize_motd.yaml 
	BECOME password: 
	
	PLAY [Prueba con variables para personalizar motd] ***************************************************************************************************
	
	TASK [Gathering Facts] *******************************************************************************************************************************
	[WARNING]: Platform linux on host debian1 is using the discovered Python interpreter at /usr/bin/python3.9, but future installation of another Python
	interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.18/reference_appendices/interpreter_discovery.html for
	more information.
	ok: [debian1]
	
	TASK [Customizar motd, mensaje de bienvenida] ********************************************************************************************************
	ok: [debian1]
	
	PLAY RECAP *******************************************************************************************************************************************
	debian1                    : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
	```

- Entramos en el host *debian1* para verificar que efectivamente el mensaje ha cambiado:

	```bash
	ssh debian1
	Linux 012a46bc2d08 6.4.0-150600.23.47-default #1 SMP PREEMPT_DYNAMIC Thu Apr  3 03:44:04 UTC 2025 (2854fd7) x86_64
	Hello squash. Here again? Do you know you are welcome? 
	Last login: Wed May 28 18:09:36 2025 from 172.18.0.1
	```

	> Hay que clarificar que para modificar el fichero **/etc/motd** son necesarios permisos de **root**, tanto tenemos que adaptar nuestro fichero **ansible.cfg** para que escale privilegios hacia root usando sudo y preguntando por la contraseña. 

- Fichero **ansible.cfg**

```ini
[defaults]
inventory = ./maquinas.yaml
deprecation_warnings=false 

[privilege_escalation]
become = true
become_method = sudo
become_user = root
become_ask_pass = true
```

##### A continuación, se va a realizar utilizando variables:

- Modificamos el playbook anterior para que el mensaje este contenido en una variable

```yaml
---
- name: Prueba con variables para personalizar motd
  hosts: debian1
  vars:
    mensaje: "Hello squash. Here again? Do you know you are welcome? You are a good sysadmin"

  tasks:
  - name: Customizar motd, mensaje de bienvenida
    copy:
      content: "{{ mensaje }}"
      dest: /etc/motd
...
```

##### A continuación, se va a realizar utilizando un array con tres elementos:

- El primer elemento será: "Hello Squash"
- El segundo elemento será: "Here again? Do you know you are welcome?"
- El tercer elemento será: "You are a good sysadmin"
-----
- En primer lugar creamos el playbook:

	```yaml
	---
	- name: Prueba con variables para personalizar motd
	  hosts: debian1
	  vars:
	    mensaje:
	      - "Hello squash"
	      - "Here again? Do you know you are welcome?"
	      - "You are a good sysadmin"
	
	  tasks:
	  - name: Customizar motd, mensaje de bienvenida
	    copy:
	      content: "{{ mensaje }}"
	      dest: /etc/motd
	...
	```

- A continuación mostramos la salida de la ejecución:

	```bash
	$ ansible-playbook customize_motd.yaml 
	BECOME password: 
	
	PLAY [Prueba con variables para personalizar motd] ***************************************************************************************************
	
	TASK [Gathering Facts] *******************************************************************************************************************************
	[WARNING]: Platform linux on host debian1 is using the discovered Python interpreter at /usr/bin/python3.9, but future installation of another Python
	interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.18/reference_appendices/interpreter_discovery.html for
	more information.
	ok: [debian1]
	
	TASK [Customizar motd, mensaje de bienvenida] ********************************************************************************************************
	changed: [debian1]
	
	PLAY RECAP *******************************************************************************************************************************************
	debian1                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
	```

Otra forma de acceder a los elementos del array sería a través su indice, es decir, llamar al array indicando la posición que queremos visualizar.

- A continuación, se describe el playbook para realizar la tarea de la manera anteriormente descrita:

	```yaml
	---
	- name: Prueba con variables para personalizar motd
	  hosts: debian1
	  vars:
	    mensaje:
	      - "Hello squash"
	      - "Here again? Do you know you are welcome?"
	      - "You are a good sysadmin"
	      - "Have a good day"
	
	  tasks:
	  - name: Customizar motd, mensaje de bienvenida
	    copy:
	      content: "{{ mensaje[0] }} {{ mensaje[1] }}. {{ mensaje[3] }}"
	      dest: /etc/motd
	...
	```

##### Realizar la tarea anterior utilizando diccionarios:

- Empresa: System & Check
- Curso: Ansible
- Nivel: Alto

- Creamos el playbook utilizando diccionarios: 

	```yaml
	---
	- name: Prueba con variables para personalizar motd
	  hosts: debian1
	  vars:
	    mensaje:
	      Empresa: System & Check
	      Curso: Ansible
	      Nivel: Alto
	
	  tasks:
	  - name: Customizar motd, mensaje de bienvenida
	    copy:
	      content: "{{ mensaje }}"
	      dest: /etc/motd
	...
	
	```

- Visualizamos solo la empresa:

	```yaml
	---
	- name: Prueba con variables para personalizar motd
	  hosts: debian1
	  vars:
	    mensaje:
	      Empresa: System & Check
	      Curso: Ansible
	      Nivel: Alto
	
	  tasks:
	  - name: Customizar motd, mensaje de bienvenida
	    copy:
	      content: "{{ mensaje.Empresa }}"
	      dest: /etc/motd
	...
	```

- A continuación mostramos la salida de la ejecución:

	```bash
	ansible-playbook customize_playbook.yaml 
	BECOME password: 
	
	PLAY [Prueba con variables para personalizar motd] ***************************************************************************************************
	
	TASK [Gathering Facts] *******************************************************************************************************************************
	[WARNING]: Platform linux on host debian1 is using the discovered Python interpreter at /usr/bin/python3.9, but future installation of another Python
	interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.18/reference_appendices/interpreter_discovery.html for
	more information.
	ok: [debian1]
	
	TASK [Customizar motd, mensaje de bienvenida] ********************************************************************************************************
	changed: [debian1]
	
	PLAY RECAP *******************************************************************************************************************************************
	debian1                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
	```

- Si entramos dentro de nuestro contenedor:

	```bash
	ssh debian1
	Linux 012a46bc2d08 6.4.0-150600.23.47-default #1 SMP PREEMPT_DYNAMIC Thu Apr  3 03:44:04 UTC 2025 (2854fd7) x86_64
	System & Check
	Last login: Sun Jun  1 17:01:15 2025 from 172.18.0.1
	```

- Ahora, vamos a crear un elemento **clave:valor** mas del diccionario donde la clave se va a llamar *Responsables* y el valor va a ser un array que contendrá los siguientes elementos:
	- Squash
	- Nacho
	- Jose

Además visualizaremos el primer responsable. 

- Creamos el playbook:

	```yaml
	---
	- name: Prueba con variables para personalizar motd
	  hosts: debian1
	  vars:
	    mensaje:
	      Empresa: System & Check
	      Curso: Ansible
	      Nivel: Alto
	      Responsables:
	        - "Squash"
	        - "Nacho"
	        - "Jose"
	
	  tasks:
	  - name: Customizar motd, mensaje de bienvenida
	    copy:
	      content: "{{ mensaje.Responsables[0] }}"
	      dest: /etc/motd
	...
	```


**END**