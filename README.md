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
##### Comando para agregar solo tra DNS cuando ya tenemos una DNS asignada a connection-name
```poweshell
nmcli connection modify iturbe +ipv4.dns 9.9.9.9
```
