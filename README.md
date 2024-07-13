# AnsibleDocker
Despliegue de playbooks de Ansible en contenedores Docker

## Parte Teórica
Una breve descripción de la parte teórica se adjunta en el siguiente link: [Mis primeros pasos con Ansible y Docker](https://docs.google.com/presentation/d/1uPaufcRsuD2Z0tQCOWaj1HXsgM-_yjY-Ns1mobmIFkE/edit?usp=sharing)

## Requisitos previos
En el computador tener instalados Docker y Ansible
Para ver si se tiene instalado estos paquetes introducir los siguientes comandos:
```
docker --version
ansible --version
```
## Instalación y Ejecución

1. Clonar el repositorio en un entorno local y trabajar dentro del directorio clonado

### Ejecución de Docker
2. Construir la imagen docker a partir del Dockerfile que se encuentra en el directorio. En la revisión del Dockerfile, notará que se agregó un usuario __root__ con password __root__
```
docker image build -t cristian/ubuntu-serverssh:v1 .
```
3. Crear y correr el contenedor Docker a partir de la imagen creada.
```
docker container run -d --name dockerssh1 -p 50022:22 cristian/ubuntu-serverssh:v1
```
4. Obtener información de la ip del contenedor, para comunicarnos con el vía SSH
```
docker container inspect dockerssh1 | grep Gateway
```
5. Suponiendo que la IP obtenida del contenedor es _172.17.0.1_, probar la conexión vía SSH
```
ssh -p 50022 root@172.17.0.1
```
6. En caso de no acceder via SSH a la IP del contenedor, una alternativa es borrar el archivo _know_hosts_ de la configuracion del ssh del computador.
```
rm -rf ~/.ssh/known_hosts
```
7. En caso de haber ejecutado el comando del paso 6, ejecutar el comando del paso 5 para acceder al contenedor via SSH

### Ejecución de Playbooks de Ansible
8. En el archivo inventory, revisar (y si es necesario editar) las variables dentro de __[docker]__ que corresponde a los contenedores Dockers que están siendo ejecutados vía ssh.
9. Ejecutar el Playbook para instalar paquetes en el contenedor que está ejecutándose.
```
ansible-playbook -i inventory packages-users.yml
```
10. Ejecutar el Playbook para obtener los usuarios. 
```
ansible-playbook -i inventory get-users.yml
```
