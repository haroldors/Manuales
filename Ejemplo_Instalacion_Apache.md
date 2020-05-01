# Manual de Instalación de Apache, PHP y driver para sql server #

Los siguientes comando fueron ejecutados en Debian 10, con el fin de instalar Apache, PHP y el Driver para MSSQL 17

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

* Luego ejecutaremos lo siguientes

`services apache2 status`

* el comando anterior nos permitirá ver el estado de Apache que se instalo y si presenta algún problema de ejecución, para poder visualizar también que ya esta entregando el servicio en nuestro navegador podemos abrir el sitio localhost en nuestra maquina o mediante la dirección ip visualizar la pagina por defecto que apache nos entrega.

* para saber cual es la dirección ip que tenemos en nuestra red privada el siguiente comando nos permitirá conocerla

`hostname -I`

* para saber cual es nuestra dirección publica que tenemos el siguiente comando nos mostrara dicha información.

`curl -4 icanhazip.com`

>_Nota: si nuestro equipo esta directamente conectado a la red publica, ambos resultados serán iguales_

---

## Configuración de Sitios alojados ##

como nuestro sitio raiz lo queremos identificar como sistemadecontrol.cl, crearemos una carpeta para alojar nuestros archivos.

para ello iremos iremos a la carpeta raiz

```
sudo cd /var/www
sudo mkdir sistemadecontrol
````
ya con nuestra carpeta creada crearemos un archivo de ejemplo para que se despliegue y reemplacé lo que apache muestra por efecto.

```
sudo nano /var/www/sistemadecontrol/index.html
```

el comando anterior utilizara el editor nano que nos permitirá agregar el siguiente contenido al archivo index.html

```html
<html>
<head>
  <title>Pagina en Desarrollo</title>
</head>
  <body>Nuestra Pagina esta en Desarrollo<br>Ya pronto podran acceder al contenido final
  </body>
</html>
```
