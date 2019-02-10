# samba
## Configuración de carpeta anónima

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
## Comprobar configuración
testparm
## Reiniciar servidor
```
sudo systemctl restart smdb
sudo systemctl restart nmbd
```


