# Cómo instalar LAMP en Debian 10 Buster #


Este Documento es una recopilacion de informacion extraida de dos link 

Link1 [https://chachocool.com/como-instalar-lamp-en-debian-10-buster/]

LInk2 [https://chachocool.com/como-instalar-phpmyadmin-en-debian-10-buster/]

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

---
# Cómo instalar phpMyAdmin en Debian 10 Buster #
---

En esta entrada veremos cómo instalar phpMyAdmin en Debian 10 Buster paso a paso. Al final de esta guía podrás administrar tu sistema de bases de datos MariaDB/MySQL tanto de forma local como remota a través de esta potente herramienta web. Con phpMyAdmin un usuario local podrá trabajar remotamente sin necesidad de activar el acceso remoto del servicio de bases de datos de tu servidor o VPS Debian 10. Además, podrás trabajar desde cualquier dispositivo conectado con un simple navegador, sin necesidad de instalar pesados clientes.
---
## Preparativos previos ##
--

Previo a la instalación de phpMyAdmin en Debian 10 conviene comprobar que PHP tiene instalados algunos módulos importantes, como php-mbstring, php-zip y php-bz2. Si no estuvieran presentes, los instalaríamos mediante apt. Antes actualizamos la lista de paquetes de los repositorios:

```
sudo apt update
sudo apt -y upgrade
sudo apt -y install php-bz2 php-mbstring php-zip
```
Una vez descargados e instalados estos módulos y sus dependencias, recargamos la configuración del servidor web o del servicio PHP, según nuestra configuración:

```
sudo systemctl reload apache2
```

---
## Cómo descargar phpMyAdmin para Debian 10 Buster ##
---
En este ejemplo elegiremos la versión 5 y el paquete con formato .tar.xz. Si estás navegando en la máquina que vas a realizar la instalación puedes descargar directamente y guardar el archivo donde quieras.

En mi caso, copiaré el enlace y realizaré la descarga desde consola mediante el comando wget:
```
wget https://files.phpmyadmin.net/phpMyAdmin/5.0.2/phpMyAdmin-5.0.2-all-languages.tar.xz
```
---
## Cómo instalar phpMyAdmin en Debian 10 Buster ##
---
Por razones de simplicidad, en esta guía vamos a instalar phpMyAdmin como una sección de la página web por defecto de Debian 10. Es recomendable tener instalado un certificado SSL para trabajar con protocolo seguro HTTPS, pero phpMyAdmin funcionará perfectamente bajo protocolo HTTP convencional.

La web por defecto de Apache reside en /var/www/html/, por lo que esta será la ruta donde descomprimiremos el paquete:

```
sudo tar xf phpMyAdmin-5.0.2-all-languages.tar.xz -C /var/www/html/
```
La carpeta que se crea tiene un nombre muy largo, lo mejor es crear un enlace simbólico más corto, o directamente renombrar la carpeta:
```
 sudo mv /var/www/html/phpMyAdmin-5.0.2-all-languages/ /var/www/html/phpmyadmin
 sudo chown www-data /var/www/html/phpmyadmin/
```

---
## Preparación de la base de datos de phpMyAdmin ##
---
Ciertas características de phpMyAdmin requieren que la aplicación disponga de su propia base de datos.

Usaremos el cliente de consola mysql para crear el usuario que manejará esta base de datos:
```
mysql -u root -p
```
Para MariaDB o MySQL 5, creamos el usuario como de costumbre:
```
> create user pma@localhost identified by 'XXXXXXXX';
```
Concedemos permisos a este usuario sobre la base de datos de phpMyAdmin:
```
> grant all privileges on phpmyadmin.* to pma@localhost;
```
Y ya podemos cerrar el cliente mysql:
```
> exit
```
Para inicializar la base de datos necesaria utilizaremos desde consola un archivo SQL previsto al efecto:
```
~$ cat /var/www/html/phpmyadmin/sql/create_tables.sql | mysql -u pma -p
```
---
## Configurar phpMyAdmin en Debian 10 Buster ##
---
Aunque phpMyAdmin ya está listo para funcionar con la configuración por defecto, pero debes saber que es posible configurar phpMyAdmin en Debian 10.

Existe una página de configuración accesible desde la carpeta setup/. En el ejemplo de esta guía sería accesible desde la URL http://debian10.local/phpmyadmin/setup/

Este configurador es accesible porque no existe aún un archivo de configuración. No es deseable dejar el configurador accesible, puesto que como ves no está protegido por contraseña, así que tienes dos opciones:

Usar un archivo mínimo de configuración por defecto.
Crear la configuración desde el configurador y guardarla.

---
### Usar la configuración por defecto ###
---
El configurador permite una enorme cantidad de posibilidades y opciones, por lo que puede que no quieras perder tiempo y anularlo directamente.

Para ello copia el archivo de configuración mínima de muestra presente en la carpeta de phpMyAdmin:

```
~$ sudo cp /var/www/html/phpmyadmin/config.sample.inc.php /var/www/html/phpmyadmin/config.inc.php
```
Editaremos este archivo para establecer un par de ajustes necesarios:

```
$cfg['blowfish_secret'] = ''; /* YOU MUST FILL IN THIS FOR COOKIE AUTH! */
```
Se trata de una clave para cifrar las cookies de sesión. Especificaremos como valor una cadena de 32 caracteres aleatorios:

```
$cfg['blowfish_secret'] = 'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX';
```
Configuramos también la conexión a la base de datos de phpMyAdmin, para lo que buscaremos esta sección:
```
/* User used to manipulate with storage */
// $cfg['Servers'][$i]['controlhost'] = '';
// $cfg['Servers'][$i]['controlport'] = '';
// $cfg['Servers'][$i]['controluser'] = 'pma';
// $cfg['Servers'][$i]['controlpass'] = 'pmapass';
```
Y activaremos las líneas del usuario y la contraseña, especificando la contraseña:
```
$cfg['Servers'][$i]['controluser'] = 'pma';
$cfg['Servers'][$i]['controlpass'] = 'XXXXXXXX';
```
También activaremos todas las variables de la sección «Storage and tables«:

```
/* Storage database and tables */
$cfg['Servers'][$i]['pmadb'] = 'phpmyadmin';
$cfg['Servers'][$i]['bookmarktable'] = 'pma__bookmark';
...
$cfg['Servers'][$i]['designer_settings'] = 'pma__designer_settings';
$cfg['Servers'][$i]['export_templates'] = 'pma__export_templates';
```
Guardamos los cambios y cerramos el archivo.

Al existir ya un archivo de configuración, el configurador queda bloqueado:

Justo lo que pretendíamos, bloqueamos el configurador, pero phpMyAdmin funciona perfectamente con los valores por defecto.

---
### Crear una configuración a medida ###
---
Si prefieres configurar a medida las características de phpMyAdmin, puedes usar el configurador y bucear entre todas sus opciones y herramientas. Dado que los valores por defecto funcionan en la mayoría de escenarios, la configuración de phpMyAdmin en Debian 10 escapa a las pretensiones de esta guía.

Lo que sí debes saber es que una vez que has realizado la configuración, NO es posible guardarla automáticamente. Lo que debes hacer es lo siguiente:

En la parte inferior de la pantalla principal del configurador dispones de un botón «Descargar«. Al pulsarlo se descargará un archivo config.inc.php.
Debes copiar ese archivo en la carpeta de phpMyAdmin, lo que se podrá hacer de una forma u otra, según si estás conectado de forma local o remota.
Una vez copiado el archivo, el configurador quedará bloqueado.