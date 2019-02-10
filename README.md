# Crear recursos compartidos SAMBA
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
## Comprobar configuración samba
```
testparm
```
## Reiniciar servidor
```
sudo systemctl restart smdb
sudo systemctl restart nmbd
```
# Creación de usuarios samba
Para que un usuario pueda utilizar samba tenemos que utilizar el comando smbpasswd. Podemos agregar un usuario existente en el sistema a samba:
```
sudo smbpasswd -a nombredelusuario
```
Del mismo modo, podemos eliminarlo de samba
```
sudo smbpasswd -x nombredelusuario
```
Para consultar los usuarios samba:
```
sudo pdbedit -v -L
```
# Conectar a recursos compartidos a través de shell
## Escanear la red en búsqueda de hosts SMB
```
sudo findsmb
```
También podríamos ver una representación textual en forma de árbol de las carpetas e impresoras compartidas por vecinos de red:
```
sudo smbtree
```
En concreto podemos listar los servicios ofrecidos por un servidor para usuarios anónimos:
```
smbclient -L IPdelServidor
```
Si queremos listar los recursos para un usuario concreto (nos pedirá su contraseña):
```
smbclient -L IPdelServidor -U nombredelusuario
```

## conexión con smbclient
Desde terminal, conectaríamos de la siguiente manera:
```
smbclient //IPdelServer/nombredelrecurso
```
Luego podemos subir archivos o descargar al estilo FTP
```
put nombrearchivo
get nombrearchivo
```
También podemos conectar y ejecutar comandos en una línea
```
smbclient //IPdelServer/nombredelrecurso -c 'put nombredearchivo'
smbclient //IPdelServer/nombredelrecurso -c 'ls'
```

# Montar un recurso compartido en nuestro sistema de archivos

Instalamos el paquete del sistema de archivos CIFS
```
sudo apt install cifs-utils
```
Creamos el punto de montaje:
```
sudo mkdir /mnt/nombredecarpeta/
```
Montamos la carpeta:
```
sudo mount -t cifs -v //IPdelServidor/nombredelrecurso /mnt/nombredecarpeta
```
A partir de aquí, una vez montada la carpeta, podemos acceder a ella en /mnt/nombredecarpeta, como si de una carpeta local se tratara. Para desmontar el recurso:
```
sudo umount /mnt/nombredecarpeta
```
