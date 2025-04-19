# Definición de variables en ficheros externos

-----
- Tags: #ansible #variables #ficheros
-----

### Definición 
-----

Son ficheros en los que se pueden definir las variables de manera que quede todo mejor organizado.
Estos ficheros, normalmente se ubican en el directorio donde se encuentra el inventario. 

Es común y de buenas prácticas realizar un árbol de directorios para realizar una buena organización. Estos directorios tienen que tener nombres específicos, ya que, [ansible](02_courses/linux/curso_linux/00_Intro.md) buscará en ellos, los ficheros necesarios para poder operar. 

- A continuación, se expone una captura del árbol de directorios común:

	```
	├── ansible.cfg
	├── group_vars              # Este seria el directorio para los grupos de variables
	│   └── servidores_web
	├── host_vars               # Este sería el directorio para las variables que afetan a los hosts
	│   └── rocky1.yaml
	├── maquinas
	└── maquinas.yaml
	```

	> Como se puede observar, los ficheros **.yaml** ubicados dentro de los directorios **group_vars** y **host_vars** llevan el mismo nombre que el host o el grupo de hosts al que vayan referidos. 

### Invetario en formato IAML
-----

- Teniendo el siguiente inventario: 

```yaml
all:
  hosts:
	ubuntu1:
	  puerto: 9000
	  entorno: desarrollo
	debian1:

  children:
	debian:
	  hosts:
		debian1:
		debian2:
	  vars:
	    ansible_use: squash # aquí pondriamos las variables
	rocky:
	  hosts:
		rocky1:
		rocky2:
	servidores_datos:
	  hosts:
		mysql1:
		mysql2:
	servidores_web:
	  hosts:
		tomcat1:
		tomcat2:
```

si utilizamos el árbol de directorios definido en el apartado anterior, tenemos: 

#### Directorio **host_vars**

Este directorio tendrá un fichero por cada host que tengamos en el inventario y al cual queramos que afecten las variables. 

- El contenido del fichero será como el mostrado a continuación:

```bash
$ tree
.
├── ansible.cfg
├── group_vars
│   └── servidores_web
├── host_vars
│   └── rocky1.yaml
├── maquinas
└── maquinas.yaml

2 directories, 5 files

$cat host_vars/rocky1.yaml 
ansible_user: usu1
```

#### Directorio **group_vars**

Este directorio tendrá un fichero por cada grupo de hosts que tengamos en el inventario y al cual queramos que afecten las variables. 

```bash
$ tree
.
├── ansible.cfg
├── group_vars
│   └── servidores_web
├── host_vars
│   └── rocky1.yaml
├── maquinas
└── maquinas.yaml

2 directories, 5 files

$ cat group_vars/servidores_web 
ansible_user: administrator
```

- Si ahora realizamos varias ejecuciones:

```bash
$ ansible rocky1 -m ping
rocky1 | UNREACHABLE! => {
    "changed": false,
    "msg": "Failed to connect to the host via ssh: usu1@rocky1: Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).",
    "unreachable": true
}
```

```bash
ansible servidores_web -m ping
tomcat1 | UNREACHABLE! => {
    "changed": false,
    "msg": "Failed to connect to the host via ssh: administrator@tomcat1: Permission denied (publickey,password).",
    "unreachable": true
}
tomcat2 | UNREACHABLE! => {
    "changed": false,
    "msg": "Failed to connect to the host via ssh: administrator@tomcat2: Permission denied (publickey,password).",
    "unreachable": true
}
```

Como podemos observar, aunque se produzca error, ya que esto sucede por que los usuarios no existen dentro del contenedor, pero si que hace todo correcto, es decir, recoge los usuarios definidos en los ficheros de variables e intenta conectarse con ellos a la máquina remota. 


**END**