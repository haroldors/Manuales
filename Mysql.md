# Apunte de Mysql #
---

La tabla user almacena toda la información referente a los usuarios, junto con sus privilegios globales. Los columnas de esta tabla se utilizan para determinar cuándo rechazar o permitir las conexiones entrantes, para cada usuario. Para las conexiones aceptadas, todos los privilegios otorgados a través de esta tabla (columnas que finalizan con la cadena "_priv") se denominan globales y aplican a todas las bases de datos en el servidor.

Si se desea listar todos los usuarios de un servidor MySQL, simplemente se debe seleccionar la columna "User" perteneciente a la tabla mysql. Por ejemplo:
```
mysql> select User from mysql.user;
+------------------+
| User             |
+------------------+
| backup_user      |
| blogger0inis     |
| busr0joomla10    |
| busr0estrate     |
| busr0testingss4  |
| nousr            |
| pusr_appstrtr3   |
| pusr_inst_user   |
| 3usr             |
| testblogusr      |
| testcccsite      |
| testjoomlausr    |
| testjoomlausr2   |
| testjoomlausr3   |
| testframework    |
| test5            |
| testnousr111111  |
| testlinuxito1    |
| test1111111111   |
| root             |
| root             |
| debian-sys-maint |
| root             |
| sustent_user     |
| testjoomlausr    |
| testjoomlausr2   |
+------------------+
26 rows in set (0.00 sec)
```

También es posible seleccionar el campo "Host", el cual indica desde qué host(s) se le permite el acceso a cada usuario.

Internamente el servidor almacena la información de privilegios en diferentes tablas de la base de datos mysql, llamadas grant tables. El servidor MySQL lee el contenido de estas tablas en memoria cuando inicia y toma las decisiones de control de acceso basándose en estas copias de dichas tablas en memoria. Por esta razón es que es necesario ejecutar el comando FLUSH PRIVILEGES (para que el servidor recargue estas tablas nuevamente en memoria) cada vez que se modifican permisos utilizando sentencias INSERT, UPDATE o DELETE.

La tabla db almacena los privilegios a nivel bases de datos. A través de la misma se determina qué usuarios pueden acceder a qué bases de datos desde qué hosts. Las columnas de privilegios determinan las operaciones permitidas. Un privilegio otorgado a nivel base de datos aplica a la base de datos y a todos los objetos en la misma, tales como tablas y procedimientos.

Si deseamos saber qué usuarios tienen alguna clase de acceso a cada base de datos, es posible cruzar las tablas user y db con la siguiente consulta SQL:

```
mysql> select u.User,Db from mysql.user u,mysql.db d where u.User=d.User;
+------------------+----------------------------------+
| User             | Db                               |
+------------------+----------------------------------+
| blogger0inst     | busr0wordpresssite2              |
| busr0joomla10    | busr0joomla10                    |
| busr0estrate     | busr0appl129437                  |
| busr0testingss4  | busr0testingss4                  |
| nousr            | pusr_website200_56000030         |
| pusr_appstrtr3   | pusr_appstrtr3                   |
| pusr_appstrtr3   | linuxito1                        |
| pusr_inst_user   | pusr_php_test                    |
| r3usr            | pusr_dumbapplication1_56000030   |
| testblogusr      | test_blogger0                    |
| testcccsite      | test_joomla10_1                  |
| testjoomlausr    | test_appstrtr3_r                 |
| testjoomlausr    | linuxito1                        |
| testjoomlausr    | linuxito1                        |
| testjoomlausr2   | linuxito1                        |
| testjoomlausr2   | test_appstrtr3_r                 |
| testjoomlausr2   | test_appstrtr3                   |
| testjoomlausr3   | test_appstrtr3                   |
| testjoomlausr3   | test_appstrtr3_r                 |
| testjoomlausr3   | linuxito1                        |
| testmframework   | test_framework                   |
| test5            | test_anothercrappysite           |
| testnousr111111  | test_icantbelievethis            |
| testlinuxito1    | test_linuxito1                   |
| test1111111111   | test_networking3                 |
| dumb_user        | busr0phpframework4               |
| testjoomlausr    | test_appstrtr3_r                 |
| testjoomlausr    | linuxito1                        |
| testjoomlausr    | linuxito1                        |
| testjoomlausr2   | linuxito1                        |
| testjoomlausr2   | test_appstrtr3_r                 |
| testjoomlausr2   | test_appstrtr3                   |
+------------------+----------------------------------+
32 rows in set (0.00 sec)
```
Luego es posible determinar qué tipo de acceso tiene un usuario en particular examinando el resto de las columnas de la tabla db.

De igual forma, es posible examinar qué usuarios tienen privilegios a nivel tablas (examinando la tabla tables_priv) y privilegios a nivel columnas (examinando la tabla columns_priv). Estas tablas son similares a la tabla db, sólo que implementan una mayor granularidad. Un privilegio a nivel tabla aplica a la tabla y todas sus columnas. Un privilegio a nivel columna aplica sólo a la columna indicada. Por otro lado, la tabla procs_priv aplica a los procedimientos almacenados (stored procedures).
