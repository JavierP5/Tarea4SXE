# Tarea4SXE

# LAMP & WordPress Setup on Ubuntu Container

Esta práctica consiste en realizar varias tareas utilizando un contenedor de Ubuntu para instalar un servidor LAMP y WordPress. A continuación, se detallan los pasos realizados con los comandos utilizados y una breve descripción de cada uno.

## 1. Descargar la imagen "ubuntu:22.04" y comprobar que está en tu equipo

- **Comandos utilizados:**
    ```bash
    docker pull ubuntu:22.04
    docker images
    ```

- **Descripción:**
    - Se descarga la imagen "ubuntu:22.04" desde Docker Hub.
    - Se verifica que la imagen se haya descargado correctamente con el comando `docker images`.

---

## 2. Crear un contenedor y arrancarlo

- **Comandos utilizados:**
    ```bash
    docker run -it --name lamp_container -p 80:80 -p 3306:3306 ubuntu:22.04 /bin/bash
    ```

- **Descripción:**
    - Se crea y se arranca un contenedor llamado 'lamp_container' en modo interactivo y se expone el puerto 80 para Apache y el puerto 3306 para MySQL.

---

## 3. Actualizar el sistema e instalar Apache

- **Comandos utilizados:**
    ```bash
    apt update
    apt install -y apache2
    service apache2 start
    ```

- **Descripción:**
    - Se actualiza la lista de paquetes y se instala el servidor Apache.
    - Se inicia el servicio de Apache.

---

## 4. Instalar MySQL

- **Comandos utilizados:**
    ```bash
    apt install -y mysql-server
    service mysql start
    mysql_secure_installation
    ```

- **Descripción:**
    - Se instala el servidor MySQL y se inicia el servicio.
    - Se ejecuta el script de seguridad para configurar MySQL.

---

## 5. Instalar PHP

- **Comandos utilizados:**
    ```bash
    apt install -y php libapache2-mod-php php-mysql
    service apache2 restart
    ```

- **Descripción:**
    - Se instala PHP y los módulos necesarios para su integración con Apache y MySQL.
    - Se reinicia Apache para aplicar los cambios.

---

## 6. Comprobar la instalación de LAMP

- **Comandos utilizados:**
    ```bash
    echo "<?php phpinfo(); ?>" > /var/www/html/info.php
    ```

- **Descripción:**
    - Se crea un archivo PHP de prueba para verificar que PHP está funcionando correctamente.
    - Se puede acceder a `http://localhost/info.php` desde el navegador.

---

## 7. Descargar e instalar WordPress

- **Comandos utilizados:**
    ```bash
    apt install -y wget unzip
    wget https://wordpress.org/latest.zip
    unzip latest.zip
    mv wordpress/* /var/www/html/
    ```

- **Descripción:**
    - Se descargan y descomprimen los archivos de WordPress en el directorio de documentos de Apache.

---

## 8. Configurar la base de datos de WordPress

- **Comandos utilizados:**
    ```bash
    mysql -u root -p
    ```

- **Descripción:**
    - Se accede a la consola de MySQL. Dentro de la consola, se ejecutan los siguientes comandos:
    ```sql
    CREATE DATABASE wordpress;
    CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'password';
    GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpressuser'@'localhost';
    FLUSH PRIVILEGES;
    EXIT;
    ```

- **Descripción:**
    - Se crea una base de datos para WordPress y un usuario con los privilegios necesarios.

---

## 9. Configurar WordPress

- **Comandos utilizados:**
    ```bash
    cp /var/www/html/wp-config-sample.php /var/www/html/wp-config.php
    nano /var/www/html/wp-config.php
    ```

- **Descripción:**
    - Se copia el archivo de configuración de ejemplo y se modifica para incluir la información de la base de datos:
    ```php
    define('DB_NAME', 'wordpress');
    define('DB_USER', 'wordpressuser');
    define('DB_PASSWORD', 'password');
    define('DB_HOST', 'localhost');
    ```

---

## 10. Cambiar permisos

- **Comandos utilizados:**
    ```bash
    chown -R www-data:www-data /var/www/html/
    chmod -R 755 /var/www/html/
    ```

- **Descripción:**
    - Se cambian los permisos de los archivos de WordPress para que Apache pueda acceder a ellos correctamente.

---

## 11. Comprobar el acceso a WordPress

- **Descripción:**
    - Accede a `http://localhost` desde el navegador y sigue las instrucciones para completar la configuración de WordPress.
