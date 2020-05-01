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
apt install php
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

* una vez guardado los cambios procederemos a recargar apache para que aplique los cambios, para esto ejecutaremos el siguiente comando.

`/etc/init.d/apache2 reload`

* Nuevamente si queremos visualizar que todo cargo ok, podemos ejecutar el siguiente comando.

`/etc/init.d/apache2 status`

>_Nota: En el caso de presentar algún error en la ejecución debería verificar los cambios aplicados en el archivo anterior._
