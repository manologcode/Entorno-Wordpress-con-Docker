# Entorno Wordpress con Docker

contenedores para wordpress con mysql y phpmyadmin. 

Este contenedor nos sirve tanto para instalar un wordpress nuevo en nuestro entorno local como para para pasar una copia de una pagina en producción a 
entorno local para actualizaciones o pruebas.

Wordpress se muestra en

http://localhost/

PhpMyAdmin se muestra en

http://localhost:8080/

## crear un nuevo Wordpress

clonar le repositorio y ejecutar docker-compose up,  parar la ejecucion y volver a ejecutar como demonio

       docker-compose up -d

abrir el navegador y correr http://localhost/ y seguir las instrucciones de instalacion

## montar una copia de Wordpress

clonar le repositorio

1. Copiar los archivos en la carpeta public

2. copiar el archivo wp-config.php dentro de la carpeta public y reemplazar el existente por este

3. acceder a la capeta public y cambiar el propietario de los archivos a www-data

   sudo chown -R www-data:www-data *

4. revisar la configuración de docker-compose.yml, sobre todo las variables de entorno ej. WORDPRESS_TABLE_PREFIX: wxuaq_ 
	y arrancar el docker-compose up -d

- Base de datos

5. entra en el phpmyadmin http://localhost:8080/  us: root | pass: toor

6. importar archivo de base de datos dentro del esquema wp_data_base

7. cambiar los datos del dominio a localhost, correr las siguiente instrucciones en la consola SQL cambia el nombre_de_dominio

```

SET @old_domain="http://nombre_de_dominio";
SET @new_domain="http://localhost";

START TRANSACTION;

	UPDATE wp_options SET option_value = replace(option_value, @old_domain, @new_domain) WHERE option_name = 'home' OR option_name = 'siteurl';

	UPDATE wp_posts SET guid = replace(guid, @old_domain, @new_domain);

	UPDATE wp_posts SET post_content = replace(post_content, @old_domain, @old_domain);

	UPDATE wp_postmeta SET meta_value = replace(meta_value, @old_domain, @old_domain);

COMMIT;

```

8. si toda ha ido bien accediendo a http://localhost/ tenemos que ver la web

y en http://localhost/wp-admin el panel de control



