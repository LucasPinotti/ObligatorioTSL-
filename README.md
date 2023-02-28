# Taller de Servidores Linux - Obligatorio

## Configuración del Bastión Rocky

1. Se utilizo como servidor bastion RockyLinux 8
2. Se instala Ansible **sudo dnf install ansible**
3. Se genero un usuario Ansible el cual realizara las ejecuciones de los Playbooks
4. Mediante ventana SSH generamos la clave pública en el equipo Bastión con **ssh-keygen**
5. Se distribuyo la clave publica en los equipos a administrar con el comando **ssh-copy-id**
6. Se instala coleccion para hardening del sistema operativo **ansible-galaxy collection install devsec.hardening**

## Configuraciones en servidores host

- Se configuraron dos servidores host, uno con sistema operativo Ubuntu y el otro Rocky. En estos se particionaron segun se indico en la letra del obligatorio, quedando de la siguiente manera:

- Estos poseen dos placas de red, una configurada en NAT la cual se utiliza para la salida de internet y otra de HostOnly para la comunicacion con el Bastion (La red con el Bastion es 192.168.190.0/24).

- Se genero en ambos equipos un usuario Ansible sin contraseña el cual tiene permiso para escalar a sudo sin password y es utilizado unicamente por el Bastion al conectarse con Ansible.

## Estructura de PlayBook

- Se armo el Playbook general llamado main.yml el cual ejecuta el proceso de hardening sobre los servidores y    luego invoca a un par de sub playbooks que se encuentran en la raiz del repositorio (app.yml y db.yml).

- El proceso de hardening se obtiene desde: <https://github.com/dev-sec/ansible-collection-hardening/tree/master/roles/os_hardening>, dado que la distribucion Rocky al realizar consultas no devuelve el nombre RedHat en la familia de SO
 se tuvo que copiar el rol y realizar modificaciones para la aplicacion.

- Los SubPlaybooks ejecutan sobre determinados hosts y ejecutan dos roles. Todos los roles estan pensados para funcionar sobre Ubuntu y Rocky sin importar el rol de cada uno.
- Roles:
    1. Common
        Actualiza los paquetes de los servidores clientes a la ultima version
        Instala paquetes genericos requeridos en las distribuciones para la gestion
    2. MariaDB
        Ejecuta la instalacion de los paquetes necesarios para correr el servicio MariaDB y realiza las configuraciones necesarias para el funcionamiento de nuestra aplicacion
        En caso de generar la base vacia en el servidor ejeuta un .sql para la generacion de las tablas
    3. Tomcat9
        Ejecuta la instalacion de Apache Tomcat y todas sus dependencias. Luego realiza toda la configuracion necesaria para el funcionamiento de la aplicacion Todo
        Instala un Apache para el acceso al tomcat y lo configura como Proxy

## Ejecucion de PlayBook
