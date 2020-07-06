# Cómo instalar LAMP en Debian 10 Buster #

---
## Instalar PHP7.4 ##

---
Debian incluye PHP 7.3 en sus repositorios pero si quieres PHP 7.4 puedes instalar el repositorio.

```
sudo apt update
sudo apt upgrade -y && sudo reboot
sudo apt -y install lsb-release apt-transport-https ca-certificates 
sudo wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/php.list
sudo apt update
sudo apt -y install php7.4
sudo apt-get install php7.4-{bcmath,bz2,intl,gd,mbstring,mysql,zip}
```

---
## Instalar MariaDB 10.4 ##
---
Debian 10 incluye la versión 10.3 en sus repositorios, así que como en el caso anterior, si necesitas la versión 10.4 tendrás que instalar el repositorio oficial:

```
sudo nano /etc/apt/sources.list.d/mariadb.org-10.4.list

```
Con esta línea:
```
deb http://ftp.ddg.lth.se/mariadb/repo/10.4/debian buster main
```
Guardamos el archivo y descargamos la clave pública para la comprobación de firmas de este repositorio:
```
curl -sSL 'http://keyserver.ubuntu.com/pks/lookup?op=get&search=0xF1656F24C74CD1D8' | sudo apt-key add -
```
---
## Instalación de los componentes de la pila LAMP ##
---

```
sudo apt update
sudo apt upgrade -y
sudo apt install -y apache2 libapache2-mod-php php-mysql mariadb-server
```
con el siguiente comando podemos comprobar el estado de ambos servicios

```
systemctl status apache2 mariadb
```
---

## Configurar el firewall para la pila LAMP en Debian 10 ##

---
```
sudo ufw allow http
```
---
## Cómo configurar el servidor LAMP en Debian 10 ##
---

Configurar al detalle cada uno de los elementos de la pila LAMP excede esta pequeña guía, sobre todo porque la instalación deja el servidor LAMP totalmente funcional. Pero unas indicaciones sobre dónde encontrar los archivos de configuración siempre vienen bien.

### El motor de base de datos ###

La ruta de los archivos de configuración de MariaDB se encuentra en /etc/mysql/. El archivo principal de configuración es my.cnf que se limita a cargar configuraciones prensentes en los directorios conf.d/ y mariadb.conf.d/.

La configuración por defecto ha eliminado las bases de datos de pruebas y los usuarios anónimos. El único usuario que existe es root con acceso local, sin clave, aunque sólo puede conectar con el cliente mysql el propio root o un usuario mediante sudo.

### PHP ###

La ruta de los arhivos de configuración de PHP es /etc/php/. Dentro de esta carpeta hay otras tantas como versiones de PHP tengamos instaladas, en ese caso sólo tendríamos 7.3/. Finalmente dentro de esta se encuentran las configuraciones organizadas por carpetas de Apache, línea de comandos, y módulos. El archivo que nos interesa es /etc/php/7.3/apache2/php.ini, ya que es el lugar donde ajustaremos cualquier configuración de PHP para las aplicaciones LAMP.

Deberías editar php.ini para asigarle el valor de tu zona horaria a la directiva date.timezone. Además, la configuración por defecto es para entorno de producción, por lo que los mensajes de error no se muestran en las páginas web, tendrías que buscarlos en los archivos de log del servidor web.

### El servidor web ###

La ruta de configuración es /etc/apache2/, siendo el archivo principal de configuración /etc/apache2/apache2.conf. Este archivo incluye las configuraciones guardadas en otras carpetas, como conf-enabled/, mods-enabled/ y sites-enabled/, relativas a configuraciones adicionales, servidores virtuales y módulos de Apache.

La página por defecto se aloja en /var/www/html/, donde puedes alojar contenido sin configuraciones adicionales.

Para más información sobre configuración, servidores virtuales, etc. puede que te interese la guía de instalación de Apache en Debian 10 Buster.

---
## Cómo probar el servidor LAMP en Debian 10 ##
---

Para comprobar el funcionamiento de nuestro nuevo servidor LAMP en Debian 10 Buster crearemos un pequeño script en PHP y lo situaremos en la carpeta de la página por defecto de Apache.

Crearemos el script, llamdo info.php, en la ruta /var/www/html/:

```
sudo nano /var/www/html/info.php
```
El contenido será una sola línea con una llamada a la función phpinfo(), que nos proporcionará muchísima información sobre la configuración de PHP:

```
<?php phpinfo(); ?>
```
Accederemos al servidor LAMP usando la dirección IP, nombre o dominio de la máquina Debian 10 y añadiendo la ruta /info.php. Por ejemplo, si la máquina es accesible con el dominio debian10.local, la URL sería http://debian10.local/info.php

En esta página podemos ver información sobre la versión de PHP, archivos de configuración, módulos cargados, etc. Por ejemplo, aparecerá información sobre los módulos cargados de MySQL que funcionan perfectamente con MariaDB.

Pero vamos a crear una prueba un poco más elaborada, creando una base de datos y un usuario en MariaDB con los que conectaremos desde una página en PHP.

Iniciamos sesión con el cliente mysql a través de sudo:

```
sudo mysql
```
Creamos la base de datos de pruebas lamp_db:

```
create database lamp_db;
Query OK, 1 row affected (0.001 sec)
```
Y ahora creamos el usuario lamp_user con contraseña 1234 al que le concederemos permisos sobre la base de datos anterior:

```
> create user lamp_user identified by '1234';
Query OK, 0 rows affected (0.016 sec)
> grant all privileges on lamp_db.* to lamp_user;
Query OK, 0 rows affected (0.001 sec)
```
Hecho esto sólo queda actualizar los permisos y salir a consola:

```
> flush privileges;
Query OK, 0 rows affected (0.001 sec)
> exit
Bye
~$
```
Ahora crearemos el sencillo script php en /var/www/html/:

```
sudo nano /var/www/html/phptest.php
```
Y el contenido será este:
```PHP
<?php
$enlace = mysqli_connect("127.0.0.1", "lamp_user", "1234", "lamp_db");
if (!$enlace) {
        echo "Error: No se pudo conectar a MariaDB/MySQL.\n";
        echo "código de error: " . mysqli_connect_errno() . PHP_EOL;
        echo "mensaje de error: " . mysqli_connect_error() . PHP_EOL;
        exit;
}
echo "<h1>Éxito: ¡Se realizó una conexión apropiada a MariaDB/MySQL!</h1>\n";
echo "<h2>Información del host: " . mysqli_get_host_info($enlace) . "</h2>\n";
mysqli_close($enlace);
```

Este script es simple, intenta conectar al sistema de bases de datos usando la función mysqli_connect() especificando la dirección del sevidor, el usuario, la contraseña y la base de datos. La dirección del servidor es 127.0.0.1 ó localhost, ya que los servidores web y de bases de datos están en la misma máquina.

Accedemos de nuevo al servidor LAMP para probar este script mediante el navegador. Siguiendo el ejemplo, la URL en esta ocasión sería http://debian10.local/phptest.php).

