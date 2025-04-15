# Variables en Ansible

En este capítulo vamos a realizar una introducción a las variables en [00_Ansible](00_Ansible.md)

-----
- Tags: #ansible #variables 
-----

### Tipos de variables
-----

En ansible, como en cualquier otro lenguaje, existen varias formas de implementar variables y también existen varios tipos de variables como pueden ser:

- Variables de entorno.
- Variables implementadas en los ficheros de inventario.
- Variables implementadas en los [01_playbooks](01_playbooks.md).
- Variables implementadas en los roles.
- Variables especiales denominadas (magic, conexión, facts)

#### Variables de entorno

- Se utilizan para definir valores en el sistema operativo, por ejemplo, **ANSIBLE_CONFIG** [01_ficheros_configuracion](01_ficheros_configuracion.md).

#### Variables de inventario

- Se utilizan en los ficheros de inventarios y se pueden aplicar a nivel de hosts o de grupo.

#### Variables de Playbook

- Son variables que pueden ser definidas a nivel de play e incluso de task

#### Variables de Role

- Son variables que se pueden utilizar dentro de Role concreto.

#### Variables especiales

Existen varios tipos de variables especiales: 

- **Magic**: Son variables que reflejan el estado interno y no son modificables.
- **Facts**: Son variables que contienen información del host, como pueden ser, el nombre del host, la dirección IP, etc.
- **Conexión**: Son variables que determinan como se ejecutan las acciones contra una máquina, como pude ser el modo de conexión, etc. Por ejemplo, **ansible_become_user**.


**END**

