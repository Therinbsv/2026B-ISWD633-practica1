## Esquema para el ejercicio
![Imagen](esquema-ejercicio3.PNG)

### Crear red net-wp
C:\Users\P3E010-D>docker network create net-wp

### Para que persista la información es necesario conocer en dónde mysql almacena la información.
# COMPLETAR LA SIGUIENTE ORACIÓN. REVISAR LA DOCUMENTACIÓN DE LA IMAGEN EN https://hub.docker.com/
En el esquema del ejercicio carpeta del contenedor (a) es /var/lib/mysql

Ruta carpeta host: .C:\Users\P3E010-D\Documents\ejercicio3\bd

### ¿Qué contiene la carpeta db del host?
Inicialmente, la carpeta db se encuentra vacía. 

### Crear un contenedor con la imagen mysql:8  en la red net-wp, configurar las variables de entorno: MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER y MYSQL_PASSWORD
C:\Users\P3E010-D>docker run -d --name KatheMysql --network net-wp -v "C:\Users\P3E010-D\Documents\ejercicio3\db:/var/lib/mysql" -e MYSQL_ROOT_PASSWORD=Root12345 -e MYSQL_DATABASE=wordpress_db -e MYSQL_USER=wp_user -e MYSQL_PASSWORD=Wp12345 mysql:8

### ¿Qué observa en la carpeta db que se encontraba inicialmente vacía?
Se observa que MySQL ha generado archivos y directorios relacionados con el almacenamiento y la administración de la base de datos. Esto demuestra que los datos se están escribiendo en la carpeta del host vinculada a /var/lib/mysql, permitiendo que persistan aunque se elimine el contenedor MySQL, siempre que se conserve correctamente esa información.

### Para que persista la información es necesario conocer en dónde wordpress almacena la información.
# COMPLETAR LA SIGUIENTE ORACIÓN. REVISAR LA DOCUMENTACIÓN DE LA IMAGEN EN https://hub.docker.com/
En el esquema del ejercicio la carpeta del contenedor (b) es (COMPLETAR CON LA RUTA)

Ruta carpeta host: .../ejercicio3/www

### Crear un contenedor con la imagen wordpress en la red net-wp, configurar las variables de entorno WORDPRESS_DB_HOST, WORDPRESS_DB_USER, WORDPRESS_DB_PASSWORD y WORDPRESS_DB_NAME (los valores de estas variables corresponden a los del contenedor creado previamente)
# COMPLETAR CON EL COMANDO

### Personalizar la apariencia de wordpress y agregar una entrada

### Eliminar el contenedor y crearlo nuevamente, ¿qué ha sucedido?

# COMPLETAR CON LA RESPUESTA A LA PREGUNTA 

