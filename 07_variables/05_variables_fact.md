# Variables fact

-----
- Tags: #ansible #variables #fact #variables_fact
-----

### Definición
-----

Estas variables son variables o propiedades que contienen información de la máquina remota. 

- Podemos ver un ejemplo con la ejecución del siguiente comando:

```bash
$ ansible -i maquinas.yaml debian1 -m setup
debian1 | SUCCESS => {
    "ansible_facts": {
        "ansible_all_ipv4_addresses": [
            "172.18.0.2"
        ],
        "ansible_all_ipv6_addresses": [],
        "ansible_apparmor": {
            "status": "enabled"
        },
        "ansible_architecture": "x86_64",
        "ansible_bios_date": "11/12/2020",
        "ansible_bios_vendor": "Phoenix Technologies LTD",
        "ansible_bios_version": "6.00",
        "ansible_board_asset_tag": "NA",
        "ansible_board_name": "440BX Desktop Reference Platform",
        "ansible_board_serial": "NA",
        "ansible_board_vendor": "Intel Corporation",
        "ansible_board_version": "None",
        "ansible_chassis_asset_tag": "No Asset Tag",
        "ansible_chassis_serial": "NA",
        "ansible_chassis_vendor": "No Enclosure",
        "ansible_chassis_version": "N/A",
        "ansible_cmdline": {
            "BOOT_IMAGE": "/vmlinuz-6.4.0-150600.23.47-default",
            "mitigations": "auto",
            "preempt": "full",
            "quiet": true,
            "resume": "/dev/vg_system/lv_swap",
            "root": "/dev/mapper/vg_system-lv_root",
            "security": "apparmor",
            "splash": "silent"
        },
```

Con la ejecución del anterior comando vemos que nos devuelve mucha información, muchas variables que estan recogiendo información de la máquina remota. **Estas variables, son variables de tipo fact**

###### Las variables fact, se pueden encontrar de tres maneras distintas:

- Variables normales definidas como **"clave"**:**"valor"**
- Variables de tipo diccionario. Es como una lista de clave - valor. Están definidas entre llaves *{}*
- Variables de tipo array. Están definidas entre corchetes *[]*

### Recuperación de variables **fact**
-----

Para recuperar el valor de alguna de las variables que nos devuelve el comando de ansible ejecutado anteriormente, es decir, 

	$ ansible -i maquinas.yaml debian1 -m setup

- ejecutamos la siguiente orden:

```bash
$ ansible -i maquinas.yaml debian1 -m setup -a 'filter=ansible_system'
debian1 | SUCCESS => {
    "ansible_facts": {
        "ansible_system": "Linux",
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false
}
```

Como podemos ver en la propiedad *ansible_system* nos indica que es un sistema de tipo *linux*

- Otro ejemplo:

```bash
$ ansible debian1 -m setup -a "filter=ansible_processor"
debian1 | SUCCESS => {
    "ansible_facts": {
        "ansible_processor": [
            "0",
            "GenuineIntel",
            "Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz",
            "1",
            "GenuineIntel",
            "Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz",
            "2",
            "GenuineIntel",
            "Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz",
            "3",
            "GenuineIntel",
            "Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz"
        ],
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false
}
```


**END**