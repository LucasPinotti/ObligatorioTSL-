# Taller de Servidores Linux - Obligatorio

## Configuración del Bastión Rocky

1. Se utilizo como servidor bastion Ubuntu 22.04 en WSL en la maquina host
2. Se instala Ansible **sudo apt install ansible**
3. Se genero un usuario Ansible el cual realizara las ejecuciones de los Playbooks
4. Mediante ventana SSH generamos la clave pública en el equipo Bastión con **ssh-keygen**
5. Se distribuyo la clave publica en los equipos a administrar con el comando **ssh-copy-id**


## Estructura de PlayBook
- Se armo el Playbook general llamado main.yml el cual ejecuta el proceso de hardening sobre los servidores y luego invoca a un par de sub playbooks que se encuentran en la raiz del repositorio (app.yml y db.yml).
- Los SubPlaybooks ejecutan sobre determinados hosts y ejecutan dos roles. Todos los roles estan pensados para funcionar sobre Ubuntu y Rocky sin importar el rol de cada uno.
- Roles:
    1. Common
        Actualiza los paquetes de los servidores clientes a la ultima version
    2. Mysql
        Ejecuta la instalacion de los paquetes necesarios para correr el servicio MySql y realiza las configuraciones necesarias para el funcionamiento de nuestra aplicacion
    3. Tomcat9
        Ejecuta la instalacion de Apache Tomcat y todas sus dependencias. Luego realiza toda la configuracion necesaria para el funcionamiento de la aplicacion Todo

