# Módulo FILE. Trabajar con ficheros y directorios

-----
- Tags: #ansible #modulo #file #modulo_file 
-----

### Definición

El modulo *file* es uno de los mas utilizados en el mundo de [ansible](00_Ansible.md)

Este modulo nos permite:

- Trabajar con ficheros, directorios, enlaces, ..., 
- Esta incluido en la colección "ansible.builtin" formando parte del núcleo o **core** de ansible. 
- Cabe destacar que hay otros módulos parecidos.

### Parámetros
-----

- **group**: Grupo al que pertenece el objeto.
- **mode**: Permisos con los que se configura el objeto. Se puede indicar en formato octal o en modo simbólico. *777*, *u+rw*.
- **owner**: Propietario del objeto.
- **state**:
	- "absent" -> Permite borrar ficheros y directorios.
	- "directory" -> Permite crear directorios incluidos los intermedios que no existan.
	- "file" -> Crear ficheros.
	- "hard" -> Crear un hard link. 
	- "link" -> Crear un soft link.
	- "touch" -> crear un fichero vacío. 


