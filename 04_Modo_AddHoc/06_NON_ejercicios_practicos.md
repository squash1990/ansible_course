# Ejercicios de Ansible 

En este apartado vamos a realizar algunos ejemplos prácticos para dejar asentado el uso de comandos *add-hoc*

### Ejemplo 1

#### Con el módulo **command** averiguar los sistemas de ficheros de la máquina **tomcat1**:

```bash
ansible@dev-lab:/home/squash/Desktop/squash/ansible/practicas> ansible -i maquinas tomcat1 -m command -a "df -h"
tomcat1 | CHANGED | rc=0 >>
Filesystem                     Size  Used Avail Use% Mounted on
overlay                         30G   12G   19G  39% /
tmpfs                           64M     0   64M   0% /dev
shm                             64M     0   64M   0% /dev/shm
/dev/mapper/vg_system-lv_root   30G   12G   19G  39% /etc/hosts
tmpfs                          1.2G   52K  1.2G   1% /run
tmpfs                          5.0M     0  5.0M   0% /run/lock
tmpfs                          587M     0  587M   0% /run/user/1000
```


#### Recuperar la memoria libre de todos los rocky: 

```bash
ansible@dev-lab:/home/squash/Desktop/squash/ansible/practicas> ansible -i maquinas rocky* -m command -a "free -h"
rocky1 | CHANGED | rc=0 >>
               total        used        free      shared  buff/cache   available
Mem:           5.7Gi       2.1Gi       1.5Gi        69Mi       2.4Gi       3.6Gi
Swap:          8.0Gi          0B       8.0Gi
rocky2 | CHANGED | rc=0 >>
               total        used        free      shared  buff/cache   available
Mem:           5.7Gi       2.1Gi       1.5Gi        69Mi       2.4Gi       3.6Gi
Swap:          8.0Gi          0B       8.0Gi
                         587M     0  587M   0% /run/user/1000
```


#### Averiguar que usuarios tienen asignada una shell *bash* en la máquina *debian1*. Utilizar el módulo **shell**

```bash
ansible@dev-lab:/home/squash/Desktop/squash/ansible/practicas> ansible -i maquinas debian1 -m shell -a "cat /etc/passwd | grep -i bash"
debian1 | CHANGED | rc=0 >>
root:x:0:0:root:/root:/bin/bash
ansible:x:1000:1000:,,,:/home/ansible:/bin/bash
```
