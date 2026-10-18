# Conocimientos logrados  
## Comparación de conocimientos 
### Antes de la práctica
Mis conocimientos en Docker se limitaban a la gestión básica de imágenes y contenedores individuales, utilizando comandos esenciales como docker run, docker start, docker stop, docker rm y el mapeo básico de puertos mediante -p para acceder a interfaces web locales. Asimismo, sabía acceder a la terminal de un contenedor mediante docker exec -it para configuraciones puntuales.
### Después de la práctica
He comprendido y aplicado conceptos avanzados de persistencia de datos y orquestación de redes en Docker, permitiendo que la información sobreviva al ciclo de vida de los contenedores. Aprendí a estructurar arquitecturas multicontenedor complejas como la integración de un servidor web Drupal con bases de datos PostgreSQL y clientes de administración tipo pgAdmin, interconectadas de forma segura bajo redes virtuales de tipo bridge.

## Lo aprendido
### 1. Bind Mounts (Montajes del Sistema de Archivos del Host)
* **Def:** Mecanismo que vincula un directorio o archivo específico de tu computadora física (*host*) directamente dentro del sistema de archivos de un contenedor. Permite editar archivos localmente y ver los cambios reflejados al instante, manteniendo los datos a salvo si el contenedor se destruye.
* **Recomendaciones y requisitos previos:**
  * Crea previamente una carpeta local en el equipo con la estructura limpia que se va a utilizar --> ej, carpetas para código fuente, recursos estáticos o configuraciones.
  * Consulta siempre la documentación oficial de la imagen en Docker Hub para identificar la ruta exacta dentro del contenedor donde espera encontrar esos archivos --> ej. `/usr/share/nginx/html`, `/var/www/html`.
* **Pasos y comandos:**
  1. Ubicarse en el equipo local y prepara el directorio físico con los archivos o plantillas que el servicio consumirá.
  2. Ejecuta el contenedor mapeando el puerto de la máquina hacia el contenedor e indicando la ruta absoluta o relativa del host seguida de dos puntos y la ruta interna del contenedor:
     ```bash
     docker run -d --name <nombre_contenedor> -p <puerto_host>:<puerto_contenedor> -v "<ruta_absoluta_host>:<ruta_contenedor>" <imagen>:<tag>
     ```
  3. Si se necesita eliminar el contenedor para liberar recursos sin perder los archivos locales, se utiliza el siguiente comando de eliminación directa:
     ```bash
     docker rm -f <nombre_contenedor>
     ```

---

### 2. Volúmenes Nombrados (*Named Volumes*) y Redes en Docker

* **Def:** Espacios de almacenamiento administrados íntegramente por el motor de Docker (separados del sistema de archivos local del usuario) combinados con redes virtuales de tipo *bridge* para interconectar múltiples servicios de forma aislada y segura.
* **Recomendaciones y requisitos previos:**
  * Diseñar un esquema previo de la arquitectura para identificar qué contenedores se comunicarán entre sí (ej. aplicación web + base de datos + cliente de administración).
  * Revisa la documentación de las imágenes para conocer las variables de entorno obligatorias como: credenciales, nombres de bases de datos por defecto, usuarios. Además de las múltiples rutas internas que requieren persistencia como carpetas de módulos, perfiles, sitios o datos de bases de datos.
* **Pasos y comandos:**
  1. Se crea una red virtual para aislar y permitir la comunicación interna entre los contenedores de la arquitectura:
     ```bash
     docker network create -d bridge <nombre_red>
     ```
  2. Crear anticipadamente los volúmenes nombrados independientes necesarios para cada componente de almacenamiento:
     ```bash
     docker volume create <nombre_volumen>
     ```
  3. Despliegue del contenedor de la base de datos (backend) conectándolo a la red creada, configurando sus variables de entorno de acceso y omitiendo la exposición de puertos hacia el exterior por seguridad:
     ```bash
     docker run -d --name <nombre_db> -e VARIABLE_DB_USER=<usuario> -e VARIABLE_DB_PASSWORD=<clave> --network <nombre_red> <imagen_db>:<tag>
     ```
  4. Despliegue de herramientas de gestión gráfica (como clientes web de administración) conectándolas a la red y exponiendo un puerto hacia el navegador:
     ```bash
     docker run -d --name <nombre_cliente> -p <puerto_host>:<puerto_contenedor> -e CREDENCIALES_ADMIN=<valor> --network <nombre_red> <imagen_cliente>:<tag>
     ```
  5. Despliegue del servidor de la aplicación principal (frontend/backend web) conectándolo a la red, exponiendo su puerto de acceso y vinculando todos los volúmenes nombrados necesarios mediante múltiples flags `-v`:
     ```bash
     docker run -d --name <nombre_app> -p <puerto_host>:<puerto_contenedor> -v <nombre_volumen_1>:<ruta_contenedor_1> -v <nombre_volumen_2>:<ruta_contenedor_2> --network <nombre_red> <imagen_app>:<tag>
     ```

---

### 3. Volúmenes Anónimos (*Anonymous Volumes*) y Limpieza de Recursos

* **Def:** Volúmenes generados de manera automática por Docker cuando configuras una ruta interna del contenedor sin especificar ni un nombre de volumen ni una ruta del host. Se emplean principalmente para datos temporales o de caché.
* **Recomendaciones y requisitos previos:**
  * Utilízalos únicamente cuando la información almacenada **no sea crítica** o solo deba persistir mientras el contenedor esté vivo.
  * Se debe acostumbrar a realizar limpiezas periódicas para evitar que los volúmenes huérfanos consuman espacio innecesario en el disco duro de tu computadora.
* **Paso a paso y comandos:**
  1. Ejecuta el contenedor indicando únicamente la ruta interna de la aplicación que requiere almacenamiento temporal:
     ```bash
     docker run -d --name <nombre_contenedor> -v <ruta_interna_contenedor> <imagen>:<tag>
     ```
  2. Al terminar de usarlo, se elimina el contenedor asegurándote de borrar también su volumen anónimo vinculado mediante la bandera de forzado y purga:
     ```bash
     docker rm -fv <nombre_contenedor>
     ```
  3. Ejecuta el comando global de mantenimiento para eliminar cualquier volumen anónimo remanente (*dangling volumes*) que haya quedado suelto en el sistema operativo:
     ```bash
     docker volume prune
     ```
  
