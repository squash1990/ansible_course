# Instalación de paquetes en debian (Práctica)

A continuación, vamos a realizar una instalación de un servidor web apache en una distribución *Debian* 

### Pasos a seguir

- En primer lugar vamos a utilizar el *módulo **apt*** para insatalar el paquete *apache2*

    `ansible -i maquinas debian1 -m apt -a "name=apache2 state=present"`

- A continuación, usamos el *módulo **service*** para verificar el estado actual del servicio. 

    `ansible -i maquinas debian1 -m service -a "name=apache2 state=started"`


**END**
