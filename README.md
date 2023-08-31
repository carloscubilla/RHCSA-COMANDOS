# RHCSA-COMANDOS

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
> En este directorio almacenamos los archivos en donde se indican permisos para usuarios o grupo de usuarios
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
