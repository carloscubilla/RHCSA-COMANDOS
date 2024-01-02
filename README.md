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

##### Comando para crear una particion en el disco sdc
```poweshell
parted /dev/sdc mkpart primary ext4 200MB 400MB
```

##### Comando para guardar los ambios y que se visualicen
```poweshell
udevadm settle
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
