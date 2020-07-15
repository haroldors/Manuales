# Este Documento permite corregir ciertos errores que se pueden presentar #
----
## MariaDB (MySQL) ##
---
### Corregir ERROR 1698 (28000): Access denied for user ‘root’@’localhost’ en MariaDB (MySQL) ###
Si tras instalar el servidor MariaDB (MySQL), e intentar acceder al servidor por consola o por phpMyAdmin con la cuenta de root, obtenemos el siguiente error:
```
$ mysql -u root -p
Enter password: 
ERROR 1698 (28000): Access denied for user 'root'@'localhost'
```
Existen varias formas de corregir el error. En este artículo, mostraremos una de ellas. Para conseguirlo, ejecutaremos los siguientes comandos y sentencias:

Tendremos que ejecutar el cliente de MariaDB (MySQL) como superusuario precedido del comando sudo:
```
$ sudo mysql -u root -p
```
Una vez que hemos accedido al servidor, seleccionamos la base de datos mysql:

```
MariaDB [(none)]> USE mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A
 
Database changed
```
Listamos el contenido de los campos user y plugin:

```
MariaDB [mysql]> SELECT user, plugin FROM user;
+------------+-------------+
| user       | plugin      |
+------------+-------------+
| root       | unix_socket |
| phpmyadmin |             |
+------------+-------------+
2 rows in set (0.00 sec)
```
Actualizamos de unix_socket a mysql_native_password:
```
MariaDB [mysql]> UPDATE user SET plugin="mysql_native_password" WHERE user="root";
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```
Comprobamos los cambios listando de nuevo los campos user y plugin:

```
MariaDB [mysql]> SELECT user, plugin FROM user;
+------------+-----------------------+
| user       | plugin                |
+------------+-----------------------+
| root       | mysql_native_password |
| phpmyadmin |                       |
+------------+-----------------------+
2 rows in set (0.00 sec)
```
Forzamos al servidor a recargar los privilegios de los usuarios, logrando que los cambios surtan efecto tras la ejecución de la sentencia sin necesidad de reiniciar el servidor:
```
MariaDB [mysql]> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)
```
Cerramos la conexión con el servidor y cerramos el cliente:
```
MariaDB [mysql]> exit;
```
Ahora, probamos sin sudo:
```
$ mysql -u root -p
```

---
## Como Habilitar L2TP en LinuxMint 18 ##
---
En el caso que LinuxMint 18 no tenga instalado los siguientes paquetes

```
network-manager-l2tp 
network-manager-l2tp-gnome
```

se debe primero verificar que la distribucion es la que se menciona con el siguiente comando

```
cat /etc/linuxmint/info
```
si corresponde la distribucion se deeb agregar el repositorio  e instalar los paquetes con los siguientes comandos
```
sudo add-apt-repository ppa:nm-l2tp/network-manager-l2tp
apt-get update
​apt-get install network-manager-l2tp network-manager-l2tp-gnome
```
---