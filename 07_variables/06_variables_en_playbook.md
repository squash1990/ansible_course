# Uso de variables en playbooks

-----
- Tags: #ansible #variables #playbook #variables_en_playbook
-----

### Uso y ejemplos
-----

#### Uso del módulo debug
-----

- En primer lugar, definimos un playbook sencillo para realizar pruebas:
	
	```yaml
	---
	- name: Prueba de playbooks con variables
	  hosts: debian1
	
	  tasks:
	  - name: Ver las variables
	    debug:
	      msg: Esto es una prueba
	...
	```

	> En este playbook, se ha definido una tarea, en la cual se hace uso del modulo **debug** que permite lanzar mensajes por pantalla, como se muestra a continuación.

- Ejecución del playbook:

	```bash
	ansible-playbook variables.yaml 
	
	PLAY [Prueba de playbooks] ****************************************************************************************************************
	
	TASK [Gathering Facts] ********************************************************************************************************************
	ok: [debian1]
	
	TASK [Ver las variables] ******************************************************************************************************************
	ok: [debian1] => {
	    "msg": "Esto es una prueba"
	}
	
	PLAY RECAP ********************************************************************************************************************************
	debian1                    : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  
	```

	> Se observa el mensaje que anteriormente se ha definido, que es visualizado por pantalla.

#### Uso de variables 

Si el mensaje definido en el playbook anterior, se quiere que este dentro de una variables para después usarla a conveniencia, la variable se define dentro del **play**, de la siguiente manera:

```yaml
---
- name: Prueba de variables en playbooks.
  hosts: debian1
  vars: { # Definimos la sección vars
    mensaje: "Esto es una prueba de uso de variables en playbooks de ansible", # Definimos el mensaje
    curso: "Curso de ansible de 0 a pro"
  }

  tasks:
  - name: Ver el contenido de las variables utilizando el módulo debug.
    debug:
      msg: "{{ mensaje }}. {{ curso }}" # Esta es la forma de utilizar la variable, encerrada entre dos llaves.
...
```


**END**