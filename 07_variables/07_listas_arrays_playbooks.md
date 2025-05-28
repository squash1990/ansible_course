# Uso de listas y arrays en playbooks

-----
- Tags: #ansible #variables #listas #arrays  
-----

### Definición
-----

Los arrays son una herramienta que proporcionan la gran mayoría de lenguajes de programación que permiten agrupar variables, datos, etc, como si fuera un vector y acceder a ellos de manera dinámica. 

### Uso y ejemplo
-----

A continuación, vamos a ver la forma de utilizar arrays en [ansible](00_Ansible.md) 

- En primer lugar, reutilizamos el fichero de variables en playbook que vimos en el apartado anterior y lo modificamos de tal manera que nos quede como se muestra en el siguiente código:

	```yaml
	---
	- name: Prueba de variables y arrays en playbooks.
	  hosts: debian1
	  vars: # Sección donde se definen las variables. Cuidado con la indexación
	    mensaje: "Esto es una prueba de uso de variables de tipo array en playbooks de ansible" # Definimos los comentarios.
	    curso: "Curso de ansible de 0 a pro"
	    entornos: # Definimos un array de variables. 
	      - desarrollo
	      - testing
	      - infraestructure
	
	  tasks:
	  - name: Visualizar el contenido de las variables de tipo array.
	    debug:
	      msg: "Entornos disponibles {{ entornos }}. Has seleccionado el entorno {{ entornos[2] }}"
	...
	```

	> Como podemos observar, se ha definido una variable de tipo *array* que en este caso se llama **entornos** la cual tiene dentro tres variables o campos, desarrollo, testing e infrastructure. Dichas variables del array tienen que ir correctamente indexadas y como vemos tienen que ir acompañadas empezando por un guión. 

Por otro lado, para acceder al array podremos llamarlo y que imprima su contenido usando su nombre y poniéndolo encerrado entre llaves **{{ entornos }}**.
En el caso de que se quiera acceder a un elemento del array, se puede hacer como en otros lenguajes, por ejemplo, python, indicando entre corchetes el número de elemento del array. 

- Si queremos acceder a un rango especifico, lo hacemos de la siguiente manera: 

```yaml
---
- name: Prueba de variables y arrays en playbooks.
  hosts: debian1
  vars: # Sección donde se definen las variables. Cuidado con la indexación
    mensaje: "Esto es una prueba de uso de variables de tipo array en playbooks de ansible" # Definimos los comentarios.
    curso: "Curso de ansible de 0 a pro"
    entornos: # Definimos un array de variables. 
      - desarrollo
      - testing
      - infraestructure

  tasks:
  - name: Visualizar el contenido de las variables de tipo array.
    debug:
      msg: "Entornos disponibles {{ entornos }}. Has seleccionado el entorno {{ entornos[0:2] }}"
...
```

Como podemos ver, usando **{{ entornos[0:2] }}** accedemos a los elementos 0 y 1 del array. 

#### Otras formas de declarar un array

- Se puede declarar un array en una sola linea como se muestra a continuación: 

```yaml
---
- name: Prueba de variables y arrays en playbooks.
  hosts: debian1
  vars: # Sección donde se definen las variables. Cuidado con la indexación
    mensaje: "Esto es una prueba de uso de variables de tipo array en playbooks de ansible" # Definimos los comentarios.
    curso: "Curso de ansible de 0 a pro"
    entornos: # Definimos un array de variables. 
      - desarrollo
      - testing
      - infraestructure
    responsables: ["squash", "sys&check", "hack"]

  tasks:
  - name: Visualizar el contenido de las variables de tipo array.
    debug:
      msg: "Entornos disponibles {{ entornos }}. Has seleccionado el entorno {{ entornos[0:2] }}. Los usuarios {{ responsables }} son administradores pero hay uno {{ responsables[1] }}"
...
```


**END**