## VOLUMEN ANÓNIMO
Un volumen anónimo es un volumen gestionado por Docker, el volumen se crea automáticamente sin un nombre específico cuando ejecutas un contenedor. Son útiles cuando deseas almacenar datos temporalmente sin preocuparte por el nombre del volumen. Docker asigna un identificador único en lugar de un nombre. Se utilizan principalmente para almacenamiento temporal. 
**Considerar**
Los datos persisten mientras el contenedor exista, pero son más difíciles de gestionar y referenciar después.

```
docker run -d --name <nombre contenedor> -v <ruta carpeta contenedor> <nombre imagen>
```
ó
```
docker run -d --name <nombre contenedor> --mount type=volume,target=<ruta carpeta contenedor> <nombre_imagen>
```
destination, dst, target: La ruta donde se monta el archivo o directorio en el contenedor.


### Para eliminar el contenedor y el volumen
```
docker rm -fv server-nginx
```
_esta instrucción elimina el volumen si éste es de tipo anónimo_

# Eliminar todos los volúmenes anónimos (dangling volumes) no utilizados
Los dangling volumes son volúmenes en Docker que no están asociados a ningún contenedor en ejecución. Estos volúmenes se crean cuando se monta un volumen en un contenedor y luego se elimina ese contenedor sin eliminar explícitamente el volumen.
```
docker volume prune
```
----------
### Práctica
```
PS C:\Users\PC-AKM> docker run -d --name ejercicio5 -v /usr/share/nginx/html nginx:alpine
...
Digest: sha256:892bb2152464f0c5d8972593a5f5afaf8054552580b699d95f8ff4af08c2d33d
Status: Downloaded newer image for nginx:alpine
cbfa9622f2f45455e7bbd6668db36f8bd3ae099cbd6094186e26fab758d08b75
C:\Users\PC-AKM> docker rm -fv ejercicio5
C:\Users\PC-AKM> docker volume prune
WARNING! This will remove anonymous local volumes not used by at least one container.
Are you sure you want to continue? [y/N] y
Deleted Volumes:
c8cfec46e8484071a1c89031fe691d6e0bc7bda8bba637d316d225a998342204
832f2583914eb7f635d56ab7be2905322b3d3793ec8c7dc2d55589240b1f1b71
Total reclaimed space: 0B
```
