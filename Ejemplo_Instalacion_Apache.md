# Manual de Instalación de Apache, PHP y driver para sql server #

El presente documento mostrara los pasos que fueron ejecutados en Debian 10, con el fin de instalar Apache, PHP y el Driver para MSSQL 17

para el caso de los sitios habilitados, se aplico la configuración para los siguientes dominios

1. sistemadecontrol.cl ( Alias: www.sistemadecontrol.cl )
2. comercialhummer.sistemadecontrol.cl ( Alias: www.comercialhummer.sistemadecontrol.cl )

Para estos sitios se aplicara la configuración para los puertos http y https con certificación Letscrypt con servicios mediante Certbot

---

## Pasos para la instalación de Apache y PHP ##

* Primero ejecutaremos lo siguiente


```
apt install libapache2-mod-php
apt install apache2
apt install php-mysql
sudo su
apt-get install curl apt-transport-https
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list
apt-get update
apt-get install -y php7.3 php7.3-dev php7.3-xml php7.3-intl
php --version
```

* Los comandos anteriores nos Instalara Apache y php como también el driver para MYSQL y al final nos mostrara la versión de PHP que quedo instalado esto nos servirá para lo que viene mas adelante.

* Luego ejecutaremos lo siguiente

`services apache2 status`

* el comando anterior nos permitirá ver el estado de Apache que se instalo y si presenta algún problema de ejecución, para poder visualizar también que ya esta entregando el servicio en nuestro navegador podemos abrir el sitio localhost en nuestra maquina o mediante la dirección ip visualizar la pagina por defecto que apache nos entrega.

* para saber cual es la dirección ip que tenemos en nuestra red privada el siguiente comando nos permitirá conocerla

`hostname -I`

* para saber cual es nuestra dirección publica que tenemos el siguiente comando nos mostrara dicha información.

`curl -4 icanhazip.com`

>_Nota: si nuestro equipo esta directamente conectado a la red publica, ambos resultados serán iguales_

---

## Configuración de Sitios alojados ##

* Como nuestro sitio raiz lo queremos identificar como sistemadecontrol.cl, crearemos una carpeta para alojar nuestros archivos.

* para ello iremos nos dirigiremos a la carpeta raíz con los siguientes comandos

```
sudo cd /var/www
sudo mkdir sistemadecontrol
````
* Ya con nuestra carpeta creada crearemos un archivo de ejemplo para que se despliegue y reemplacé lo que apache muestra por efecto.

```
sudo nano /var/www/sistemadecontrol/index.html
```

* El comando anterior utilizara el editor nano que nos permitirá agregar el siguiente contenido al archivo index.html

```html
<html>
<head>
  <title>Pagina en Desarrollo</title>
</head>
  <body>Nuestra Pagina esta en Desarrollo<br>Ya pronto podran acceder al contenido final
  </body>
</html>
```

* Con lo anterior ya podemos proceder a modificar el archivo 000-default.conf , donde modificaremos el path y la información del sitio por defecto, para ello ejecutaremos el siguiente comando.

`nano /etc/apache2/sites-available/000-default.conf`

* Encontraremos varias líneas de código de las cuales las que presentan el símbolo # se encuentran comentadas y el servidor no las esta considerando en la ejecución.
comentamos el #DocumentRoot /var/www/html
y agregaremos la instrucción con nuestro path actualizado donde debería quedar de la siguiente manera

```
ServerAdmin info@brainiac.cl
ServerName sistemadecontrol.cl
ServerAlias www.sistemadecontrol.cl
#DocumentRoot /var/www/html
DocumentRoot /var/www/sistemadecontrol
```
* procedemos a guardar y ahora iremos a exportar desde Github el sitio completo creado para el subdominio comercialhummer el cual alojaremos en la raiz contenedora, para esto debemos aplicar los siguientes comandos

```
cd /var/www
apt install git
git clone https://github.com/haroldors/BrainiacV08.git
cd /etc/apache2/sites-available/
cp 000-default.conf comercialhummer.sistemadecontrol.cl.conf
nano comercialhummer.sistemadecontrol.cl.conf
```
* Ahora modificaremos la información contenida por lo siguiente

```
ServerAdmin info@brainiac.cl
ServerName comercialhummer.sistemadecontrol.cl
ServerAlias www.comercialhummer.sistemadecontrol.cl
#DocumentRoot /var/www/html
DocumentRoot /var/www/BrainiacV08
DirectoryIndex index.php
<Directory /var/www/BrainiacV08>
        Options -Indexes
</Directory>
```

* una vez guardado los cambios procederemos a recargar apache para que aplique los cambios, para esto ejecutaremos el siguiente comando.

`/etc/init.d/apache2 reload`

* Nuevamente si queremos visualizar que todo cargo ok, podemos ejecutar el siguiente comando.

`/etc/init.d/apache2 status`

>_Nota: En el caso de presentar algún error en la ejecución debería verificar los cambios aplicados en el archivo anterior._

----

## Instalación de Driver para MSSQL ##

* Para instalar el driver ejecutaremos varias líneas de comando, las cuales aconsejo ejecutarlas una a una y no todas, de una vez. Considere que antes de aplicarlo que los siguientes comandos esta considerando que la versión de PHP es la 7.3 en el caso de tener otra versión instalada debe modificar el comando antes de aplicarlo

  1. primero instalaremos los prerrequisitos

      ```
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/mssql-release.list
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql17
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
sudo apt-get install unixodbc-dev
sudo apt-get install libgssapi-krb5-2
sed -i 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/g' /etc/locale.gen
locale-gen
```

  2. Instalaremos el driver PHP para Microsoft SQL Server

      ```
sudo apt-get install unixodbc-dev
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
printf "; priority=20\nextension=sqlsrv.so\n" > /etc/php/7.3/mods-available/sqlsrv.ini
printf "; priority=30\nextension=pdo_sqlsrv.so\n" > /etc/php/7.3/mods-available/pdo_sqlsrv.ini
exit
sudo phpenmod -v 7.3 sqlsrv pdo_sqlsrv
```


---

## Habilitación de certificación Cerbot y puerto https ##

* Instalaremos Certbot

`sudo apt-get install certbot python-certbot-apache`

* Ejecutaremos el siguiente comando para obtener un certificado y hacer que Certbot edite su configuración de Apache automáticamente para servirlo, activando el acceso HTTPS en un solo paso.

`sudo certbot --apache`


---

---

>**Nota:** La información contenida en este documento es un extractó de la información contenida en los siguientes Link


[Link: Drivers Microsoft SQL Server](https://docs.microsoft.com/en-us/sql/connect/php/installation-tutorial-linux-mac?view=sql-server-ver15#installing-the-drivers-on-debian-8-9-and-10)

[Link: Prequerisitos de Drivers SQL Sever](https://docs.microsoft.com/en-us/sql/connect/php/installation-tutorial-linux-mac?view=sql-server-ver15#installing-the-drivers-on-debian-8-9-and-10)
