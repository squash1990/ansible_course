# Realización de playbooks

A continuación, vamos construir paso a paso un playbook. 

-----
- Tags: #ansible #playbook #play #tasks #fichero_configuración
-----

### Construcción de Playbook
-----

- Realizamos el primer *playbook*:
	
	```yaml
	---
	- name: Primer play de el curso.
	  hosts: all
	
	  tasks:
	  - name: Realizar un ping
	    ansible.builtin.ping:
	...
	```

- Ejecutamos el playbook mediante el uso de comandos que explicamos en la sección anterior [playbooks](01_playbooks.md).

	```bash
	$ ansible-playbook ping.yaml
	
	PLAY [Primer play de el curso.] **********************************************************************************************************************
	
	TASK [Gathering Facts] *******************************************************************************************************************************
	ok: [debian2]
	ok: [debian1]
	ok: [tomcat2]
	ok: [rocky1]
	ok: [rocky2]
	ok: [tomcat1]
	ok: [mysql1]
	
	TASK [Realizar un ping] ******************************************************************************************************************************
	ok: [tomcat2]
	ok: [debian1]
	ok: [debian2]
	ok: [rocky2]
	ok: [rocky1]
	ok: [mysql1]
	ok: [tomcat1]
	
	PLAY RECAP    *******************************************************************************************************************************************
	debian1                    : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	debian2                    : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	mysql1                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	rocky1                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	rocky2                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	tomcat1                    : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	tomcat2                    : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	****
	```

###### Vamos a explicar que esta realizando en cada fase: 

- Fase **PLAY Primer play de el curso**:
  En este punto lo que esta haciendo es indicando el nombre del playbook que va a ejecutar

- Fase **TASK Gathering Facts**
  Aquí esta recopilando hechos de los servidores remotos. Esta preguntando el estado y las propiedades de *ansible* dentro de las máquinas a las cuales lanzamos nuestro playbook.

- Fase **TASK Realizar un ping**
  En esta fase se va a ejecutar la tarea que se haya indicado en el playbook, que en este caso es realizar un ping. Si nos fijamos podemos observar que no devuelve el famoso *pong* esto es porque esta tarea no se esta ejecutando en modo interactivo si no que se esta ejecutando, digamos, en segundo plano, con el fin de que se pueda automatizar. 

- Fase **PLAY RECAP** 
  Fase donde se indica lo que se ha hecho en la ejecución del playbook. Se indica paso a paso si se realizan cambios, si hay errores, etc. 


### Añadir nueva *task*
-----

A continuación, vamos a añadir una nueva tarea, vamos a proceder a crear un fichero aleatorio en el directorio */tmp*

- Modificamos el anterior playbook: 

	```yaml
	---
	- name: Primer playbook del curso modificado
	  hosts: all
	
	  tasks:
	  - name: Emitir un ping a todas las máquinas
	    ansible.builtin.ping:
	  - name: Crear un fichero
	    ansible.builtin.shell:
	      touch /tmp/fichero1.txt
	...
	``` 

- Ejecutamos el playbook: 

	```bash
	$ ansible-playbook ping.yaml 
	
	PLAY [Primer playbook del curso modificado] **********************************************************************************************************
	
	TASK [Gathering Facts] *******************************************************************************************************************************
	ok: [tomcat2]
	ok: [debian2]
	ok: [debian1]
	ok: [rocky1]
	ok: [rocky2]
	ok: [tomcat1]
	ok: [mysql1]
	
	TASK [Emitir un ping a todas las máquinas] ***********************************************************************************************************
	ok: [debian2]
	ok: [debian1]
	ok: [tomcat2]
	ok: [rocky2]
	ok: [rocky1]
	ok: [tomcat1]
	ok: [mysql1]
	
	TASK [Crear un fichero] ******************************************************************************************************************************
	changed: [debian2]
	changed: [tomcat2]
	changed: [debian1]
	changed: [rocky1]
	changed: [rocky2]
	changed: [mysql1]
	changed: [tomcat1]
	
	PLAY RECAP *******************************************************************************************************************************************
	debian1                    : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	debian2                    : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	mysql1                     : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	rocky1                     : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	rocky2                     : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	tomcat1                    : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	tomcat2                    : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	``` 

	> Como podemos ver en la penúlitma fase, indica que se han hecho cambios en todas las máquinas y en la última fase nos indica, si se han hecho cambios, si ha habido errores, etc.

### Añadir nuevo *play*
-----

A continuación, se procede a añadir un nuevo **play**. Esto es añadir una nueva sección dentro de nuestro playbook que pueden contener varias tareas. 

#### Añadir el *play*

En primer lugar añadimos el nuevo play: 

- Modificamos el playbook: 

	```yaml
	---
	- name: Primer playbook del curso modificado
	  hosts: all
	
	  tasks:
	  - name: Emitir un ping a todas las máquinas
	    ansible.builtin.ping:
	  - name: Crear un fichero
	    ansible.builtin.shell:
	      touch /tmp/fichero1.txt
	
	- name: Instalar Nginx
	  hosts: debian
	
	  tasks:
	  - name: Parar Apache2
	    ansible.builtin.service:
	      name: apache2
	      state: stopped
	  - name: Instalar Nginx
	    ansible.builtin.apt:
	      name: nginx
	      state: present
	      update_cache: true
	  - name: Iniciar Nginx
	    ansible.builtin.service:
	      name: nginx
	      state: started
	...
	```

	> Como podemos ver nuestro nuevo play tiene varias tareas:
	> - En primer lugar paramos el servicio anterior, Apache2, ya que si no, se pueden solapar.
	> - Seguidamente, instalamos *nginx*.
	> - Por último, iniciamos el servicio de *nginx*

#### Ejecutamos el playbook

Antes de ejecutar el playbook, hay que tener en cuenta que para realizar las tareas de iniciar/parar un servicio en el sistema o instalar/desinstalar, se necesitan permisos elevados por lo que se necesita hacer uso de usuario *root*, por ejemplo, con **sudo**

Para ello, hay que modificar, de manera correcta, el fichero [ansible.cfg](01_ficheros_configuracion.md)

- Añadimos los parámetros correctos al archivo **ansible.cfg**

	```
	[defaults]
	inventory = ./maquinas.yaml
	
	[privilege_escalation]
	become = True
	become_method = sudo 
	become_user = root
	become_ask_pass = True
	```

- A continuación, ejecutamos el playbook

	```bash
	$ ansible-playbook ping.yaml 
	BECOME password: 
	
	PLAY [Primer playbook del curso modificado] **********************************************************************************************************
	
	TASK [Gathering Facts] *******************************************************************************************************************************
	ok: [debian1]
	ok: [debian2]
	ok: [tomcat2]
	ok: [rocky2]
	ok: [rocky1]
	ok: [mysql1]
	ok: [tomcat1]
	
	TASK [Emitir un ping a todas las máquinas] ***********************************************************************************************************
	ok: [tomcat2]
	ok: [debian2]
	ok: [debian1]
	ok: [rocky2]
	ok: [rocky1]
	ok: [mysql1]
	ok: [tomcat1]
	
	TASK [Crear un fichero] ******************************************************************************************************************************
	changed: [debian2]
	changed: [debian1]
	changed: [tomcat2]
	changed: [rocky1]
	changed: [rocky2]
	changed: [mysql1]
	changed: [tomcat1]
	
	PLAY [Instalar Nginx] ********************************************************************************************************************************
	
	TASK [Gathering Facts] *******************************************************************************************************************************
	ok: [debian1]
	ok: [debian2]
	
	TASK [Parar Apache2] *********************************************************************************************************************************
	changed: [debian2]
	changed: [debian1]
	
	TASK [Instalar Nginx] ********************************************************************************************************************************
	changed: [debian1]
	changed: [debian2]
	
	TASK [Iniciar Nginx] *********************************************************************************************************************************
	changed: [debian2]
	changed: [debian1]
	
	PLAY RECAP *******************************************************************************************************************************************
	debian1                    : ok=7    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	debian2                    : ok=7    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	mysql1                     : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	rocky1                     : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	rocky2                     : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	tomcat1                    : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
	tomcat2                    : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
	```


**END**
