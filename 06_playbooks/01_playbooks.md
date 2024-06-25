# Playbooks

A continuación vamos a análizar y la estructura y uso de los playbooks que se consideran la base del objetivo de [Ansible](../00_Intro/00_Ansible.md).

-----
- Tags: #ansible #playbooks
-----

### Definición
-----

Un playbook es un tipo de fichero que se utiliza a modo de plantilla permitiendonos ejecutar todo tipo de comandos, tareas, entornos complejos dentro de ansible de manera automatizada. 

- El formato de estos ficheros es **YAML**
- Son ficheros que empiezan con tres guiones **---** y terminan con tres **...**
- Las cadenas de caracteres las podemos meter entre comillas simples o dobles.  

### Estructura
-----

Aunque hay diferentes secciones dentro de un **playbook** podemos decir que están compuestos de tareas que se pueden combinar en un componente denomiado *play* y a su vez varios plays conforman un ***playbook***

- Dentro de los playbooks podemos tener los siguientes componentes o acciones:

    - ***hosts***    -> Máquinas o nodos donde se va a aplicar el playbook.
    - ***vars***     -> Variables del play.
    - ***Tasks***    -> Lista de tareas a ejecutar en el Play
    - ***Handlers*** -> Tareas que se ejecutan solo ante algún cambio.
    - ***Roles***    -> Roles a ser importados. Cargan recursos de forma ordenda

- Ejemplo de playbook: 

    ```yaml
    ---
    - name: Actaliza Servidores Apache # Nombre del playbook
      hosts: webservers                # Selección del patrón de ejecutición
      remote_user: root                # usuario remoto que queremos utilizar.

      tacks:                           # Definición de tareas
      - name: Apache actualizado       # Descripción de la tarea 1
        ansible.builtin.yum:           # Módulo a usar para actualizar
         name: httpd                   # Nombre del paquete
         state: latest                 # Estado deseado del paquete
      - name: Fichero modificado       # Tarea 2
        ansible.builtin.template:      # Uso de plantilla de fichero 
         src: /srv/httpd.j2            # Plantilla fuente del fichero
         dest: /etc/httpd.conf         # Destino del fichero
    ...  
    ```

- Las tareas se ejecutan en orden y de manera idempotente, es decir, que si no hay nada que 
hacer, no lo hará.

- Se intentan evitar los módulos command, shell  y raw, ya que estos son los únicos que no son idempotentes, es decir, se ejecutarán si o si.

- Lo lógico es que cada **playbook** realice una tarea concreta. Si hace falta crear más playbooks, se crean. 

### Uso y parámetros
-----

- En primer lugar para comprobar la sintaxis de un fichero **YAML** podemos hacer uso del comando:

	`ansible-playbook -syntax-check playbook.yaml`

- Para ejecutar un playbook usaremos la siguiente sintaxis

    `ansible-playbook nombre_playbook.yaml`

##### Hay parámetros que podemos configurar tanto en el archivo **ansible.cfg** con dentro de un **playbook** y lo haremos dentro de los datos principales. Estos son: 

 - name: nombre del playbook
 - hosts: utiliza patrones para ver en qué nodos debe ejecutarse el playbook
 - remote_user: define el usuario remoto a utilizar
 - become: tiene la misma funcionalidad que en el **ansible.cfg**
 - become_method: tiene la misma funcionalidad que en el **ansible.cfg**
 - become_user: tiene la misma funcionalidad que en el **ansible.cfg**

##### Existen otras maneras de ejecutar un playbook, como en el modo paso a paso, comprobación, etc...

- Ejecución paso a paso

	`ansible-playbook --step fichero.yaml`

- O en modo comprobación **haciendo uso del flag -C**

	`ansible-playbook -C fichero.yaml`
