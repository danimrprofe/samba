# samba
## Compartir las carpetas home
Configuración de smb.conf
```
[homes]
Comment = Carpetas home de usuarios
Browseable = yes
Read only = no
Create mask = 0700
Directory mask = 0700
valid users = %S
```
Se puede acceder a través de \\IPdelServer\nombre, donde nombre es el nombre del usuario con el que queremos acceder. Nos llevaría a la carpeta /home/nombre
## Compartir una carpeta públicamente

Crear carpeta en el servidor
```
mkdir -p /srv/samba/public
```
Configuración de permisos
```
sudo chown -R nobody.nogroup /srv/samba/public
sudo chmod -R 777 /srv/samba/public
```
Configuración de smb.conf
```
[isos]
  comment = carpeta compartida pública ISOs de SO
  path = /srv/samba/public
  browsable = yes
  directory mask = 0777
  writable = yes
  guest ok = yes
```
Se puede acceder a través de \\IPdelServer\isos
Donde nombre es el nombre del usuario con el que queremos acceder.
Nos llevaría a /srv/samba/public
## Comprobar configuración
testparm
## Reiniciar servidor
```
sudo systemctl restart smdb
sudo systemctl restart nmbd
```


