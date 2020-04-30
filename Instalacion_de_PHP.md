# Manual de Instalacion de apache y PHP con conector para mysql y mssql en Linux #

A continuacion realizaremos el paso a paso para instalar en debian
el servidor apache donde habilitaremos un sitio realizado en php para este ejemplo donde alojaremos en el carpeta home del usuario nuestros archivos del proyecto desarrollado en php
para esto como ejemplo nuestra carpeta esta en el siguiente path

/home/brainiac/Documentos/BrainiacV08

el path anterior es solo como ejemplo y ustedes deberán cambiarlo de acuerdo al path que usted tengan en su equipo

## Instalacion de Apache ##

A continuación detallaremos el paso a paso considerando que tiene acceso al modo administrador para esto en el caso de debian o ubuntu debe dar acceso al modo su y luego ingresar la password de administrador

* para la instalación de apache debemos ingresar el siguiente comando

```
apt install libapache2-mod-php
apt install apache2
```

* Luego con el siguiente comando limpias la pantalla

`clear`

* Luego con el siguiente comando verificas que este instalado ok el apache y también visualizas la versión instalada

`php --version`

* con el siguiente comando habilitaremos los parámetros de configuración ya que crearemos el archivo php.ini utilizando en el archivo que viene cargado por defecto

`cp /usr/lib/php/7.3/php.ini-production.cli /etc/php/7.3/cli/php.ini`

* con el siguiente comando podemos editaremos la configuración en el archivo para visualizar los errores

`nano /etc/php5/apache2/php.ini`

**Buscar (ctrl+w) la línea:**

_display_errors = Off_

**Sustituirla por:**

_display_errors = On_

* Luego se debe Guardar el archivo y reiniciar el servicio de Apache, para esto se debe ejecutar lo siguiente:

`sudo /etc/init.d/apache2 restart`

* para evitar que nuestro proyecto muestre el contenido de los directorios, debe editar el siguiente archivo

`nano /etc/apache2/apache2.conf`

* el siguiente contenido se le debe agregar al archivo indicado el punto anterior

```html
<Directory /var/www/ruta>
  Options -Indexes
</Directory>
```

* Luego reiniciaremos apache para que aplique las modificaciones que realizamos

`/etc/init.d/apache2 restart`

* Luego visualizaremos el estado de apache para asegurarnos que no aparezca ningún error

`/etc/init.d/apache2 status`

* Luego limpiaremos la pantalla para poder continuar

`clear`

* Luego editamos en archivo del sitio por defecto para poder redireccionar a la carpeta donde tenemos los archivos que estamos trabajando y que podemos editar sin ser root, lo cual editamos con el siguiente comando

`nano /etc/apache2/sites-available/000-default.conf`

una vez editado debemos modificar las linea que indican lo siguiente

`DocumentRoot /var/www/html`

dejándola comentada con el símbolo # y luego agregando el path de la carpeta que contiene nuestro proyecto quedando como ejemplo lo siguiente

```html
#DocumentRoot /var/www/html
DocumentRoot /home/brainiac/Documentos/BrainiacV08
```

* con el comando siguiente podemos ver que archivos contienen el path que debemos configurar correspondientes a la configuración de apache, si encontramos mas de uno aplicamos el paso anterior

`grep -R "/var/www" /etc/apache2`

 * en mi caso se aplico el punto anterior  a los siguientes archivos
```
nano /etc/apache2/sites-available/000-default.conf
nano /etc/apache2/sites-available/default-ssl.conf
nano /etc/apache2/sites-enabled/000-default.conf
nano /etc/apache2/apache2.conf
```
* luego limpiamos la pantalla  con el comando siguiente

`clear`

* Luego reiniciamos el servicio apache con el siguiente comando

`/etc/init.d/apache2 restart`

* Con el siguiente comando podemos revisar si algo que modificamos en los puntos anteriores nos esta provocando un error

`/etc/init.d/apache2 status`

## INSTALACION DE PHP 7.3 ##

a continuacion instalaremos php 7.3 de acuerdo a los siguientes paso

* Primero aplicaremos el siguiente comando

`apt-get install php7.3`

* Luego reiniciaremos el servicio apache para que se aplique los cambios anteriores

`/etc/init.d/apache2 restart`

* Luego verificaremos si quedo algún error tras lo aplicado anteriormente

`/etc/init.d/apache2 status`

* Luego a la carpeta de nuestro proyecto le aplicaremos los permisos necesarios para que pueda ser mostrado en forma online

`chmod -R 777 /home/brainiac/Documentos/BrainiacV08`

* ahora podemos visualizar nuestro sitio accediendo al nuestra url local

http://localhost

## INSTALACION DE DRIVER MYSQL ##

* Luego con el siguiente comando verificas que se instale los modulos para conexión a mysql

`apt-get install php-mysql`

* Luego reiniciaremos el servicio apache para que se aplique los cambios anteriores

`/etc/init.d/apache2 restart`

* Luego verificaremos si quedo algún error tras lo aplicado anteriormente

`/etc/init.d/apache2 status`



## INSTALACION DE DRIVER MSSQL ##
Para instalar este controlador debemos aplicar los siguientes comandos

* El siguiente comando agrega mssql

```
sudo apt-get install unixodbc-dev
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
printf "; priority=20\nextension=sqlsrv.so\n" > /etc/php/7.3/mods-available/sqlsrv.ini
printf "; priority=30\nextension=pdo_sqlsrv.so\n" > /etc/php/7.3/mods-available/pdo_sqlsrv.ini
```
* Luego reiniciaremos el servicio apache para que se aplique los cambios anteriores

`/etc/init.d/apache2 restart`

* Luego verificaremos si quedo algún error tras lo aplicado anteriormente

`/etc/init.d/apache2 status`




http://somebooks.es/configurar-la-interfaz-atom-espanol/
