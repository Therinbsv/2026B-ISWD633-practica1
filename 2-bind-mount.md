# BIND MOUNT
En un bind mount mapeamos (montar) un directorio o archivo específico del sistema de archivos del host con una parte del sistema de ficheros del contenedor.

```
docker run -d --name <nombre contenedor> -v <ruta carpeta host>:<ruta carpeta contenedor> <imagen> 
```
ó
```
docker run -d --name <nombre contenedor> --mount type=bind,source=<ruta carpeta host>,target=<ruta carpeta contenedor> <imagen>
```
- destination, dst, target: La ruta donde se monta el archivo o directorio en el contenedor.
- source, src: El origen del montaje.
  
### En tu computador crear una carpeta llamada nginx y dentro de esta carpeta crea otra llamada html. Como se aprecia en la figura.
![Volúmenes](directorio.PNG)

### Crear un contenedor con la imagen nginx:alpine, mapear todos por puertos, para la ruta carpeta host colocar el directorio en donde se encuentra la carpeta html en tu computador y para la ruta carpeta contenedor: /usr/share/nginx/html (esta ruta se obtiene al revisar la documentación de la imagen)
![Volúmenes](volumen-host.PNG)
# COMPLETAR CON EL COMANDO

### ¿Qué sucede al ingresar al servidor de nginx?
Al ingresar a http://localhost:8080, Nginx muestra el contenido de la carpeta html del computador, que está montada en el directorio /usr/share/nginx/html del contenedor. Si la carpeta está vacía, no se visualizará la página de bienvenida habitual de Nginx y se puede obtener un error 403 Forbidden.

### ¿Qué pasa con el archivo index.html del contenedor?
El archivo index.html que existía originalmente en el directorio del contenedor queda oculto mientras la carpeta del host esté montada sobre /usr/share/nginx/html. Docker no lo elimina de la imagen; simplemente deja de estar visible en esa ruta durante el montaje. Nginx utiliza los archivos presentes en la carpeta del computador.

### Ir a https://html5up.net/ y descargar un template gratuito, descomprirlo dentro de tu computador en la carpeta html
### ¿Qué sucede al ingresar al servidor de nginx?
Al ingresar nuevamente a http://localhost:8080, Nginx muestra la página web de la plantilla descargada que fue: Multiverse by HTML5, siempre que el archivo principal se encuentre directamente en la carpeta html y se llame index.html. Los cambios realizados en los archivos del computador se reflejan en el servidor sin necesidad de reconstruir la imagen ni crear otro contenedor.

### Eliminar el contenedor
docker rm -f mi_nginx

### ¿Qué sucede al crear nuevamente un contenedor montado al directorio definidos anteriormente?
# COMPLETAR CON LA RESPUESTA A LA PREGUNTA


