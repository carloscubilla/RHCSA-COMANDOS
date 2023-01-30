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
##### Comando para agregar una interfaz de red
```poweshell
nmcli connection add con-name iturbe type ethernet ifname enp7s0
```
