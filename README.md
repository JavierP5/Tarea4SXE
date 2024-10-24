# Tarea4SXE

# LAMP & WordPress Setup on Ubuntu Container

Este archivo detalla los pasos para configurar un servidor LAMP (Linux, Apache, MySQL, PHP) y WordPress en un contenedor de Ubuntu 22.04 utilizando Docker, con la opción de instalar phpMyAdmin.

```bash
# Descargar la imagen de Ubuntu y crear el contenedor
docker pull ubuntu:22.04
docker run -it --name lamp_container -p 80:80 -p 3306:3306 ubuntu:22.04 /bin/bash

# Actualizar el sistema e instalar Apache
apt update
apt install -y apache2
service apache2 start

# Instalar MySQL
apt install -y mysql-server
service mysql start
mysql_secure_installation

# Instalar PHP
apt install -y php libapache2-mod-php php-mysql
service apache2 restart

# Verificar la instalación de LAMP
echo "<?php phpinfo(); ?>" > /var/www/html/info.php
# Acceder a http://localhost/info.php para verificar que PHP está funcionando

# Descargar e instalar WordPress
apt install -y wget unzip
wget https://wordpress.org/latest.zip
unzip latest.zip
mv wordpress/* /var/www/html/

# Configurar la base de datos de WordPress
mysql -u root -p
# Dentro de MySQL ejecutar:
# CREATE DATABASE wordpress;
# CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'password';
# GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpressuser'@'localhost';
# FLUSH PRIVILEGES;
# EXIT;

# Configurar el archivo de configuración de WordPress
cp /var/www/html/wp-config-sample.php /var/www/html/wp-config.php
nano /var/www/html/wp-config.php
# Modificar las siguientes líneas en wp-config.php:
# define('DB_NAME', 'wordpress');
# define('DB_USER', 'wordpressuser');
# define('DB_PASSWORD', 'password');
# define('DB_HOST', 'localhost');

# Cambiar los permisos de los archivos
chown -R www-data:www-data /var/www/html/
chmod -R 755 /var/www/html/

# Comprobar el acceso a WordPress accediendo a http://localhost

# (Opcional) Instalar phpMyAdmin
apt install -y phpmyadmin
ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin
service apache2 restart
# Acceder a http://localhost/phpmyadmin para verificar que phpMyAdmin está funcionando

# Comandos adicionales útiles
# Para detener el contenedor:
docker stop lamp_container

# Para iniciar el contenedor nuevamente:
docker start lamp_container

# Para acceder al contenedor:
docker exec -it lamp_container /bin/bash
