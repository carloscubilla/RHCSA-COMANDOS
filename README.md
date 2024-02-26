# RHCSA-COMANDOS

#### Iniciar el sistema operativo en modo seguro rescate o emergencia y cambiar contraseña de root

##### 1- Reiniciar el sistema operativo y presionar f10

##### 2- Precionar la tecla "e" para editar el grub de inicio
![imageninicio](/img/img1.png)


##### 3- Agregamos "rd.break" al final de la siguiente linea
> Se indica en imagen

![imagen2](/img/img2.png)

##### 4- Presiona las teclas "ctrl + x" para guardar los cambios, el sistema iniciara en modo de emergencia, como podemos ver en la siguiente imagen

![imagen3](/img/img3.png)

##### 5- Realizamos un remontaje del sysroot en modo lectura y escritura, utilizamos el siguiente comando
```poweshell
mount -o remount,rw /sysroot
```

###### 6- Iniciamos un enorno controlado para realizar el cambio de contraseña, utilizamos el siguiente comando
```poweshell
chroot /sysroot
```
##### 7- Luego insertamos el comando para cambiar contraseña
```poweshell
passwd
```
![imagen3](/img/img4.png)


#### 8- Cambiamos la contraseña y nos quedaria un ultimo paso que es un reetiquetado del sistema al momento del siguiente reinicio
```poweshell
touch /.autorelabel
```
#### 9- Por ultimo salimos del entorno controlado con el comando "exit# y reiniciamos el sistema con el comando "reboot#

![imagen5](/img/img5.png)


## NMCLI - REDES

##### Comando para editar archivo de red
```poweshell
nmcli
```
##### Comando para ver el estado de los puertos
```poweshell
nmcli device status
```
##### Comando para ver conexiones
```poweshell
nmcli connection show
```
##### Comando para ver conexiones activas
```poweshell
nmcli connection show --active
```
##### Comando para ver manual de NMCLI
```poweshell
man nmcli
```
##### Comando para ver ejemplos de NMCLI
```poweshell
man nmcli-examples 
```
##### Comando para agregar una interfaz de red y que se asigne de forma automatica la ip
```poweshell
nmcli connection add con-name iturbe type ethernet ifname enp7s0
```
##### Comando para modificar una interfaz de red
```poweshell
nmcli connection modify con-name iturbe type ethernet ifname enp7s0
```
##### Comando para eliminar una interfaz de red
```poweshell
nmcli connection delete iturbe
```
##### Comando para agregar una interfaz de red y asignar ip que queremos
```poweshell
nmcli connection add con-name iturbe type ethernet ifname enp7s0 ip4 192.168.33.30/24 gw4 192.168.33.1
```
##### Comando para agregar una interfaz de red y asignar ip que queremos con DNS
```poweshell
nmcli connection add con-name iturbe type ethernet ifname enp7s0 ip4 192.168.33.50/24 gw4 192.168.33.1 ipv4.dns 192.168.33.30
```
##### Comando para agregar una interfaz de red y asignar ip que queremos 2 DNS
```poweshell
nmcli connection add con-name iturbe type ethernet ifname enp7s0 ip4 192.168.33.50/24 gw4 192.168.33.1 ipv4.dns 192.168.33.30 +ipv4.dns 192.168.33.31
```
##### Comando para agregar solo 1 DNS mas a nuestra connection-name
```poweshell
nmcli connection modify iturbe ipv4.dns 8.8.8.8
```
##### Comando para agregar  otra DNS cuando ya tenemos una DNS asignada a connection-name
```poweshell
nmcli connection modify iturbe +ipv4.dns 9.9.9.9
```
##### Verificar el autoconect
```poweshell
nmcli connection modify iturbe connettion.autoconnect no
```
> Esto desactiva el autoconnect (Lo recomendable es tenerlo desconectado)
```poweshell
nmcli connection modify iturbe connettion.autoconnect yes
```
> Esto activa el autoconnect


##### Path en donde se encuentra el archivo de configuracion de red
```poweshell
cd /etc/sysconfig/network-scripts
```
##### Configuracion de nombres de host y resolucion de nombres
```poweshell
hostname
```

#### Comandos para agregar repositorios

##### Como agregar un repositorio local en un apache httpd

##### 1. Instalar apache
```poweshell
yum install -y httpd
```

##### 2. Iniciar apache y hacer que inicie con el sistema.
```poweshell
systemctl start http ; systemctl enable --now httpd
```

##### 3. Agregar regla al firewall para publicar nuestra web
```poweshell
firewall-cmd --permanent --add-service=http
```

```poweshell
firewall-cmd --reload
```

##### 4. Descargar repositorio appstream y baseos
```poweshell
/usr/bin/reposync --repoid=appstream -p /var/www/html/repos/ --downloadcomps --download-metadata
```

```poweshell
/usr/bin/reposync --repoid=baseos -p /var/www/html/repos/ --downloadcomps --download-metadata
```

##### Comando para instalar yum-utils
```poweshell
dnf install yum-utils
```
##### Comando para agregar un repositorio
> Esto agrega el repo y por defecto lo deja disable
```poweshell
yum-config-manager --add-repo=http://rhel9master.labrhel.com/repos/base.repo
```
##### Comando para activar el repositorio
```poweshell
yum-config-manager --enable base
```


### LISTAR SERVICIOS Y VERIFICARLOS CON "SYSTEMCTL"

##### Comando para listar los socket y servicios con todos sus estados
```poweshell
systemctl list-units-files
```

##### Comando para listar los servicios con todos sus estados
```poweshell
systemctl list-units -t service
```

##### Comando para listar socket con  estados activos 
```poweshell
systemctl list-unit-files -t socket
```
##### Comando para listar socket con todos sus estados
```poweshell
systemctl list-unit-files -t socket --all
```
##### Comando para listar dependencias de un servicio
```poweshell
systemctl list-dependencies httpd
```


##### Comando para ver cual es el target por defecto.
```poweshell
systemctl get-default
```
##### Comando para desabilitar la interfaz grafica si es que instalamos como server with GUI
```poweshell
systemctl isolate multi-user.target
```
##### Comando para habilitar la interfaz grafica.
```poweshell
systemctl isolate graphical.target
```
##### Comando para cambiar el target por defecto
```poweshell
systemctl get-default <presionar TAB 2 veces>
```




> Sirve para ver el nombre del host
##### Path en donde se ubica el nombre del host
```poweshell
cat /etc/hostname
```
> No se debe editar manualmente
##### Comando para cambiar el nombre del host
```poweshell
hostnamectl set-hostname <name>
```
> Sirve para ver el nombre del host
##### Comando para que se visualice el cambio de host
```poweshell
bash
```
##### Comando para buscar resolucion de nombre y comprobar que esté bien
```poweshell
getent hosts <name>
```
##### Comando para ver ip del server <name>
```poweshell
ping -c2 <name>
```
### Enmascaramiento de servicio
##### Comando para enmascarar un servicio
```poweshell
systemctl mask sshd.service
```

##### Comando para enmascarar un servicio
```poweshell
systemctl unmask sshd.service
```

### CREACION DE PARTICIONES CON "parted" (Es mas dificil)

##### Comando para listar discos (nos dice como esta particionado y el tamaño)
```poweshell
lsblk
```

##### Comando para ver mas opciones del sistema de archivos
```poweshell
lsblk -fp
```
```poweshell
lsblk -fs
```
##### Comando para ver las particiones y los tipos de particiones
```poweshell
cat /proc/partitions
```

##### Comando para ver los tamaños de cualquier particion que seleccionemos
```poweshell
parted /dev/sda print
```

##### Comando para ver las particiones y los tipos de particiones en MB
```poweshell
parted /dev/sda unit MB print free
```

##### Comando para asignar el tipo de particionamiento para el disco, en este caso gpt
```poweshell
parted /dev/sdb mklabel gpt
```

##### Comando para asignar el tipo de particionamiento para el disco, en este caso msdos
```poweshell
parted /dev/sdb mklabel msdos
```

##### Comando para crear una particion en el disco sdc
```poweshell
parted /dev/sdc mkpart primary ext4 200MB 400MB
```

##### Comando para guardar los ambios y que se visualicen
```poweshell
udevadm settle
```
##### Comando para guardar los ambios y que se visualicen
> Es casi lo mismo que "udevadm settle" y a veces es mas efectivo
```poweshell
partprobe -s
```

##### Comando para establecer el tipo de partición lvm.
```poweshell
parted /dev/vdb set 2 lvm on
```

##### Comando para para cambiar el tamaño del LV.
```poweshell
lvextend -L 768M /dev/serverb_01_vg/serverb_01_lv
```
##### Comando para para cambiar el tamaño del LV.
> Usamos -r al final para que haga el rezize automaticamente
```poweshell
lvextend -L 768M /dev/serverb_01_vg/serverb_01_lv -r
```
##### Comando para para cambiar el tamaño del LV.
> Funciona al igual que la opcion -r
```poweshell
xfs_growfs /storage/data1
```





### CREACION DE PARTICIONES CON "fdisk" 
> Tener en cuenta que si la particion tiene Disklabel msdos se utiliza el comando "fdisk" si es Disklabel gpt se utiliza otro comando

##### Comando para editar o hacer particiones msdos
```poweshell
fdisk /dev/sdc
```

##### Comando para editar o hacer particiones gpt
```poweshell
gdisk /dev/sdc
```


##### Comando para editar fstab
```poweshell
 vim /etc/fstab
```
> Una vez que editamos y agregamos todo a nuestro sistema de archivos utilizamos el siguiente comando para cargar la configuracion

```poweshell
systemctl daemon-reload
```

##### Montamos el sistema de archivos y verificamos que no haya errores
```poweshell
mount -a
```

##### Comando para darle un label a la particion nueva
```poweshell
xfs_admin -L <xfs.nombre> /dev/sdb1
```

##### Comando para crear un volumen fisico (phisical Volume)
```poweshell
pvcreate /dev/sdb1
```

##### Comando para crear un grupo de vulumenes (Volume group)
```poweshell
vgcreate carlos1 /dev/sdb1 /dev/sdb2
```

##### Comando para crear un volumen de grupo con phisical extend diferente a 4M
```poweshell
vgcreate -s 8m vg_data /dev/sdb1 /dev/sdb2
```

##### Comando para crear un volumen logico (logical volume)
```poweshell
lvcreate carlos1 -n datos_carlos -L 700M
```

##### Comando para extender un volumen logico (logical volume)
> Primero hay que tener en cuenta que debemos asignar volumenes fisicos al grupo de volumen
> La opcion -t hace un test  nos dice si esta todo correcto o hemos cometido un error
```poweshell
lvextend /dev/vg_data/vg_data1 -L +10 -r -t
```

## COMO EXTERDER MEMORIS SWAP

##### Comando para CREAR volumenes GPT para la memoria swap
```poweshell
gdisk
```
##### Comando para guardar los ambios y que se visualice lo creado
```poweshell
udevadm settle
```

##### Comando para extender el grupo de volumenes 
```poweshell
vgextend rhel /dev/sdb1 /dev/sdb2
```

##### Comando para dejar en cero o apagar la memoria swap
> Se utiliza -v para visializar la salida del comando
```poweshell
swapoff -v /dev/rhel/swap
```

##### Comando para extender un volumen logico (logical volume)
> Primero hay que tener en cuenta que debemos asignar volumenes fisicos al grupo de volumen
> La opcion -t hace un test  nos dice si esta todo correcto o hemos cometido un error no realiza los cambios
```poweshell
lvextend /dev/rhel/swap -l +100%free -r -t
```

> Tambien se pueden utilizar las opciones para agregar por ejemplo mas 500M a la memoria
```poweshell
lvextend /dev/rhel/swap -l +500M -r -t
```
> O setear la memoria a 500M 
```poweshell
lvextend /dev/rhel/swap -l 500M -r -t
```

## CREAR VOLUMENES CON STRATIS

##### Comando para un volumen por defecto xfs con stratis
```poweshell
stratis pool create pool1 /dev/sdb
```
##### Comando extender un volumen  xfs  stratis
```poweshell
stratis pool add-data  pool1 /dev/sdc
```
##### Comando para crear un filesystem para un volumen xfs con stratis
```poweshell
stratis filesystem create pool1 file_system1
```
##### Comando para crear un snapshot del filesystem para un volumen xfs con stratis
```poweshell
stratis filesystem snapshot pool1 file_system1 snapshot1_file_system1
```
##### Comando listar el UUID del volumen xfs con stratis
```poweshell
lsblk --output=UUID /dev/stratis/pool1/file_system1
```
##### Comando para montar el volumen creado con stratis en /dir_stratis
```poweshell
echo "UUID=2f7a87d8-769c-4e2b-ae44-96d9832b4085 /dir_stratis xfs defaults,x-systemd.requires=stratisd.service 0 0" >> /etc/fstab
```

##### Comando para crear archivos de 2 GB
```poweshell
dd if=/dev/urandom of=/dir_stratis/archivo2.txt bs=1M count=2048
```

Programación de un trabajo de usuario diferido







# REVISION DE ARCHIVOS SYSLOG
##### Comando para crear una linea de log del tipo user.debug
```poweshell
logger -p user.debug "Debug Message Test"
```









# GESTION DE ARCHIVOS

## Gestión de archivos tar comprimidas

##### Comando para comprimir un archivo gz
```poweshell
tar -czf /tmp/etc.tar.gz /etc
```
##### Comando para comprimir un archivo bz2 
```poweshell
tar -cjf /tmp/etc.tar.gz /etc
```
##### Comando para comprimir un archivo xz 
```poweshell
tar -cJf /tmp/etc.tar.gz /etc
```


##### Comando para comprimir un archivo con cualquier extension.
> Use el sufijo del archivo para determinar el algoritmo que se usará.
```poweshell
tar -caf /tmp/etc.tar.gz /etc
```




##### Comando para un test para descomprimir un archivo 
```poweshell
tar -tzf /tmp/etc.tar.gz
```

##### Comando para descomprimir un archivo 
```poweshell
tar -xzf /tmp/etc.tar.gz
```
##### Comando para sincronizar archivos o directorios 
> Se utiliza "get" para obtener y "put" para enviar los archivos
```poweshell
rsync -av root@servera:/etc /configsync
```


#Ajuste del rendimiento del sistema
##### Comando para instalar tuned
```poweshell
dnf install tuned
```
##### Comando para  enumerar todos los perfiles de ajuste disponibles
```poweshell
tuned-adm list
```
##### Comando tuned-adm profile_info para obtener información sobre un perfil determinado.
```poweshell
tuned-adm profile_info network-latency
```
##### Comando tuned-adm profile profilename para cambiar a un perfil diferente que se adapte mejor a los requisitos de ajuste actuales del sistema.
```poweshell
tuned-adm profile throughput-performance
```
##### Comando tuned-adm recommend puede recomendar un perfil de ajuste para el sistema. El sistema usa este mecanismo para determinar el perfil predeterminado después de la instalación.
```poweshell
tuned-adm recommend
```

##### Comando para revertir los cambios de configuración que aplica el perfil actual, cambie a otro perfil o desactive el daemon ajustado. Desactive la actividad de ajuste de la aplicación tuned con el comando tuned-adm off.
```poweshell
tuned-adm off
```
```poweshell
tuned-adm active
```
##### Comando para ajustar el nivel de nice de cada proceso en 10. Use los valores de PID correctos para sus procesos de la salida del comando anterior.
```poweshell
sudo renice -n 10 <pid> <pid>
```

# CONTROL DE CONTEXTOS DE ARCHIVO DE SELinux
##### Comando para ver en que estado se encuentra SElinux
```poweshell
getenforce
```
##### Comando para cambiar el estado de SElinux
> usage:  setenforce [ Enforcing | Permissive | 1 | 0 ]
```poweshell
setenforce 0
```
##### Comando para DEFINIR una regla de contextos de archivos de SELinux que defina el tipo de contexto en httpd_sys_content_t para el directorio /custom y todos los archivos en él.
```poweshell
semanage fcontext -a -t httpd_sys_content_t '/custom(/.*)?'
```
##### Comando para ver el contexto de SELinux para los directorios
```poweshell
ls -dZ /custom
```
##### Comando para CORREGIR los contextos de archivos en el directorio /custom.
```poweshell
restorecon -R /custom
```
# ***

##### Comando para ajustar la política de SELinux con booleanos
```poweshell
setsebool httpd_enable_homedirs on
```
##### Comando para para ver si algún booleano restringe el acceso a los directorios para el servicio httpd.
```poweshell
getsebool -a | grep home
```
##### Comando para verifica el ajuste de la política de SELinux con booleanos
```poweshell
semanage boolean -l | grep httpd_enable_homedirs
```
##### Comando para verifica el ajuste de la política de SELinux con booleanos (Resultao igual al anterior comando pero sin usar grep)
```poweshell
semanage boolean -l -C
```
##### Comando para buscar mensaje de errores de que arroja SElinux
> Buscar /sealert
```poweshell
less /var/log/messages
```
##### Comando para ejecutar lo sugerido por SElinux
```poweshell
sealert -l 35c9e452-2552-4ca3-8217-493b72ba6d0b
```

##### Comando leer lo que hay en /var/log/audit/audit.log
> La opción -m busca en el tipo de mensaje. La opción ts busca en función del tiempo. La siguiente entrada identifica el proceso relevante y el archivo que causa la alerta
```poweshell
ausearch -m AVC -ts recent
```


# ACCESO AL ALMACENAMIENTO CONECTADO A LA RED.

##### Comando para instalar autofs
```poweshell
dnf install autofs
```
























 


## GESTION DE USUARIOS LOCALES

##### Comando para cambiar de usuario
```poweshell
su - cmcf
```

##### Comando para elevar privilegios a root
```poweshell
sudo -i
```
##### Comando para agregar usuarios
```poweshell
useradd cmcf
```
##### Comando poner contraseña a un usuario
```poweshell
passwd cmcf <press enter>
```
##### Comando para agregar usuarios al grupo wheel
```poweshell
usermod -aG wheel cmcf
```
##### Comando para crear archivo en el directorio /etc/sudoers.d/
> En este directorio almacenamos los archivos en donde se indican permisos para usuarios o grupo de usuarios, en este ejemplo es para el grupo "sysadmin"
```poweshell
echo "%sysadmin ALL=(ALL) ALL" >> /etc/sudoers.d/sysadmin
```
##### Comando para bloquear un usuario
```poweshell
usermod -L cmcf
```
##### Comando para desbloquear un usuario
```poweshell
usermod -U cmcf
```
##### Comando para establecer la vigencia máxima de la contraseña del usuario 
> El comando de abajo establece una vigencia maxima de 30 dias a la contraseña del usuario cmcf
```poweshell
chage -M 30 cmcf
```
##### Comando para un cambio de contraseña en el primer inicio de sesión en la cuenta 
> El comando de abajo fuerza el cambio de contraseña en el primer inicio de sesion del usuario cmcf
```poweshell
chage -d 0 cmcf
```
##### Comando para determinar la fecha de vencimiento en 180 dias a partir de la fecha del sistema operativo
> El comando imprime la fecha actual + los dias que queremos de vigencia
```poweshell
date -d "+180 days" +%F
```

##### Comando para que caduque en la fecha que se muestra en el paso anterior. 
```poweshell
chage -E 2022-09-06 operator1
```
##### Comando para verificar que la fecha de vencimiento de la cuenta se haya establecido correctamente
```poweshell
chage -l cmcf
```












##### Comando para verificar la zona horaria
```poweshell
timedatectl
```
##### Comando para actualizae la zona horaria a America/Port-au-Prince.
```poweshell
sudo timedatectl set-timezone America/Port-au-Prince
```
##### Comando para sincronizar la hora del sistema con el servidor classroom.example.com como la fuente de hora de NTP.
> Se agrega "server classroom.example.com iburst"    (iburst  para acelerar la sincronización de tiempo inicial)
```poweshell
vim /etc/chrony.conf
```
##### Comando para habilitar la sincronización de tiempo en la máquina servera.
> El comando activa el servidor NTP con la configuración modificada en el archivo de configuración /etc/chrony.conf. El comando puede activar el servicio chronyd o el servicio ntpd según lo que esté instalado actualmente en el sistema.
```poweshell
sudo timedatectl set-ntp true
```
##### Comando para verificar que la máquina servera esté sincronizando actualmente sus ajustes de hora con la fuente de hora de classroom.example.com.
> En el resultado se muestra un asterisco (*) en el campo de estado de la fuente (S) para la fuente de hora de NTP classroom.example.com. El asterisco indica que la hora del sistema local está sincronizada de forma correcta con la fuente de hora de NTP.
```poweshell
chronyc sources -v
```






# MANEJO DE CONTENEDORES PODMAN

##### Comando para instalar el metapaquete de contenedores
```poweshell
dnf install containet-tools
```

##### Comando para listar contenedores que se estan ejecutando
```poweshell
podman ps
```
##### Comando para listar contenedores que se estan ejecutando y tambien que no se estan ejecutando
```poweshell
podman ps -a
```

##### Comando muestra la información de configuración de la utilidad podman, incluidos sus registros configurados.
```poweshell
podman info
```
##### Comando para mostrar una lista de imágenes en los registros configurados que contienen el paquete python-38.
```poweshell
podman search python-38
```
##### Comando para examinar diferentes formatos de imagen de contenedor desde un directorio local o un registro remoto sin descargar la imagen.
```poweshell
skopeo inspect docker://registry.access.redhat.com/ubi8/python-38
```
##### Comando para descargar la imagen seleccionada en la máquina local.
```poweshell
podman pull registry.access.redhat.com/ubi8/python-38
```
##### Comando para mostrar las imágenes locales.
```poweshell
podman images
```
##### Comando para compilar la imagen. La sintaxis para el comando podman build es la siguiente:
```poweshell
podman build -t NAME:TAG DIR
```

##### Comando para ver la información de bajo nivel de la imagen de contenedor y verificar que su contenido coincida con los requisitos del contenedor.
```poweshell
podman inspect localhost/python36:1.0
```


##### Comando para inciar el contenedor.
```poweshell
podman start <container_name>
```

##### Comando para crear y ejecutar el contenedor más tarde. El comando podman run ejecuta un proceso dentro de un contenedor y este proceso inicia el nuevo contenedor.
```poweshell
podman run -d --name python38 \
registry.access.redhat.com/ubi8/python-38 \
sleep infinity
```



##### Comando para ejecutar un comando en un contenedor en ejecución.
```poweshell
podman exec python38 ps -ax
```


##### Comando para copiar archivos y carpetas entre los sistemas de archivos del host y del contenedor.
```poweshell
podman cp /tmp/hello.sh python38:/tmp/hello.sh
```


##### Comando para ejecutar desde el contenedor.
```poweshell
podman exec python38 bash /tmp/hello.sh
```


##### Comando para detener un contenedor.
```poweshell
podman stop python38
```

##### Comando para eliminar el contenedor.
```poweshell
podman rm python38
```


##### Comando para eliminar contenedores e imágenes.
> Tener en cuenta que no debe estar corriendo ningun contenedor con esa imagen.
```poweshell
podman rmi registry.access.redhat.com/ubi8/python-38
```




