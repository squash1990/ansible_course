# Instalación de Ansible y requerimentos

### Instalar python3 y pip
-----

- Para instalar **python3** ejecutamos el siguiente comando
    ```bash
    sudo apt install python3 
    ```

    > Es interesante verificar si tenemos pip3 instalado 

- Verificamos que tenemos pip instalado y que versión con la siguiente orden
    ```bash
    $ python3 -m pip -V
    pip 22.0.2 from /usr/lib/python3/dist-packages/pip (python 3.10)
    ```

### Instalamos Ansible 

1) #### Instalación mediante el sistema operativo 

    En esta sección mostramos como instalar ansible que dependerá de la distribución de linux que estemos utilizando. 
    Decir que es posible que no se instale la última versión aunque cuando se actualice ansible y actualicemos el sistema operativo, este se actualizará 

- Si tenemos distribuciones basadas en debian, utilizaremos:

    `sudo apt install update`

    `sudo apt install ansible`

    - En el caso de que no encuentre el paquete de ansible tendremos que añadir el repositorio concreto:

        `sudo add-apt-repository --yes --update ppa:ansible/ansible`
        
    - Seguidamente volvemos a ejecutar los anteriores comandos

- Si tenemos la distribución fedora:

    `sudo dnf install ansible`

- Si tenemos distribuciones basadas en CentOS:
    
    `sudo yum install epel-release`

    `sudo yum install ansible`

2) #### Instalación mediante PIP

    En esta sección vamos a ver como instalarlo mediante la herramienta PIP
    
- Instalamos la versión global de ansible utilizando pip3 mediante en siguiente comando:

    `python3 -m pip install ansible `

#### Comprobamos la versión de ansible instalada

- Una vez instalado, ejecutamos el siguiente comando para ver la versión instalada 
    ```bash
    squash@11LAP8PYJ2T3:~$ ansible --version
    ansible [core 2.16.7]   
        config file = None
        configured module search path = ['/home/squash/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
        ansible python module location = /home/squash/.local/lib/python3.10/site-packages/ansible
        ansible collection location = /home/squash/.ansible/collections:/usr/share/ansible/collections
        executable location = /home/squash/.local/bin/ansible
        python version = 3.10.12 (main, Nov 20 2023, 15:14:05) [GCC 11.4.0] (/usr/bin/python3)
        jinja version = 3.1.4
        libyaml = True
    ```

#### Instalación de Ansible a nivel local de usuario

- Para realizar este tipo de instalación ejecutamos el siguiente comando
   
    `python3 -m pip install ansible --user`


    > Cuando instalamos ansible usando este tipo de instalación **local** el instalador lo deja dentro del directorio: 
    
    `.local/bin/ansible`


**END**





    



