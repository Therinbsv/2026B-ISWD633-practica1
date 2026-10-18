## Esquema para el ejercicio
![Imagen](esquema-ejercicio3.PNG)

### Crear red net-wp
C:\Users\P3E010-D>docker network create net-wp

### Para que persista la información es necesario conocer en dónde mysql almacena la información.
# COMPLETAR LA SIGUIENTE ORACIÓN. REVISAR LA DOCUMENTACIÓN DE LA IMAGEN EN https://hub.docker.com/
En el esquema del ejercicio carpeta del contenedor (a) es /var/lib/mysql

Ruta carpeta host: C:\Users\PC-AKM\Documents\ejercicio3\db

### ¿Qué contiene la carpeta db del host?
Inicialmente, la carpeta db se encuentra vacía. 

### Crear un contenedor con la imagen mysql:8  en la red net-wp, configurar las variables de entorno: MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER y MYSQL_PASSWORD
docker run -d --name mySQL --network net-wp -v "C:\Users\PC-AKM\Documents\ejercicio3\db:/var/lib/mysql" -e MYSQL_ROOT_PASSWORD=root12345 -e MYSQL_DATABASE=wordpress -e MYSQL_USER=kathe -e MYSQL_PASSWORD=707 mysql:8

### ¿Qué observa en la carpeta db que se encontraba inicialmente vacía?
Se observa que MySQL ha generado archivos y directorios relacionados con el almacenamiento y la administración de la base de datos. Esto demuestra que los datos se están escribiendo en la carpeta del host vinculada a /var/lib/mysql, permitiendo que persistan aunque se elimine el contenedor MySQL, siempre que se conserve correctamente esa información.

### Para que persista la información es necesario conocer en dónde wordpress almacena la información.
# COMPLETAR LA SIGUIENTE ORACIÓN. REVISAR LA DOCUMENTACIÓN DE LA IMAGEN EN https://hub.docker.com/
En el esquema del ejercicio la carpeta del contenedor (b) es /var/www/html

Ruta carpeta host: C:\Users\PC-AKM\Documents\Nueva carpeta\ejercicio3\www

### Crear un contenedor con la imagen wordpress en la red net-wp, configurar las variables de entorno WORDPRESS_DB_HOST, WORDPRESS_DB_USER, WORDPRESS_DB_PASSWORD y WORDPRESS_DB_NAME (los valores de estas variables corresponden a los del contenedor creado previamente)
C:\Users\PC-AKM> docker run -d --name wordpress --network net-wp -p 9500:80 -v "C:\Users\PC-AKM\Documents\ejercicio3\www:/var/www/html" -e WORDPRESS_DB_HOST=mySQL:3306 -e WORDPRESS_DB_USER=kathe -e WORDPRESS_DB_PASSWORD=707 -e WORDPRESS_DB_NAME=wordpress wordpress

### Personalizar la apariencia de wordpress y agregar una entrada

### Eliminar el contenedor y crearlo nuevamente, ¿qué ha sucedido?

Al eliminar el contenedor de WordPress y volverlo a crear, la información no se ha perdido. El tema que se había personalizado y la entrada que se había publicado siguen ahí. Porque la información visual y los archivos de WordPress se guardaron en la carpeta www de tu computadora, mapeada al volumen /var/www/html, y los datos de las entradas y usuarios se guardaron en la base de datos MySQL, la cual a su vez guarda su información en la carpeta db de la computadora. Al destruir el contenedor, los archivos en el disco duro quedan intactos, y el nuevo contenedor simplemente los vuelve a leer al iniciar.

