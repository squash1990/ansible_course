# Ejercicios de Ansible 

En este apartado vamos a realizar algunos ejemplos prácticos para dejar asentado el uso de comandos *add-hoc*

### Ejemplos

#### Con el módulo **command** averiguar los sistemas de ficheros de la máquina **tomcat1**:

```bash
$ ansible -i maquinas tomcat1 -m command -a "df -h"
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
$ ansible -i maquinas rocky* -m command -a "free -h"
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

#### Averiguar que usuarios tienen asignada una shell *bash* en la máquina *debian1*. Utilizar el módulo **shell**:

```bash
$ ansible -i maquinas debian1 -m shell -a "cat /etc/passwd | grep -i bash"
debian1 | CHANGED | rc=0 >>
root:x:0:0:root:/root:/bin/bash
ansible:x:1000:1000:,,,:/home/ansible:/bin/bash
```

#### Copiar el fichero */etc/netconfig* al directorio */tmp* de la máquina ubuntu1:

```bash
$ ansible -i maquinas ubuntu1 -m copy -a "src=/etc/netconfig dest=/tmp"
ubuntu1 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": true,
    "checksum": "36852ec5ce61da5aebd8f23ecd2f17434f457367",
    "dest": "/tmp/netconfig",
    "gid": 1000,
    "group": "ansible",
    "md5sum": "ca8db53e3af4d735335c2607d21c7195",
    "mode": "0664",
    "owner": "ansible",
    "size": 767,
    "src": "/home/ansible/.ansible/tmp/ansible-tmp-1744132250.8826926-15202-227246860359762/source",
    "state": "file",
    "uid": 1000
}
```

#### Instalar GIT en la máquina "mysql1":

```bash
$ ansible -i maquinas mysql1 -m shell -a "sudo apt update"
$ ansible -i maquinas mysql1 --become --ask-become-pass -m apt -a "name=git state=present"
```

#### Instalar e iniciar *apache2* en la máquina *debian1*:

```bash
$ ansible -i maquinas debian1 --become --ask-become-pass -m apt -a "name=apache2 state=present"
$ ansible -i maquinas debian1 --become --ask-become-pass -m service -a "name=apache2 state=restarted"
```


**END**

