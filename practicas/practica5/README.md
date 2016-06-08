##Práctica 5. Replicación de bases de datos MySQL


El objetivo de esta práctica es poder tener un "margen de maniobra" cuando perdamos un servidor de base de datos. Para ello vamos a utilizar 2 servidores, uno principal y otro secundario. El principal mantendra la base de datos y el segundo realizará copias del primero. Vamos a hacerlo de dos maneras, la primera usando mysqldump y la segunda, automatizando el proceso para que cada sentencia que se introduzca en el servidor principal se replique en el secundario al instante. Vamos a usar una Filosofía *Master-Slave*

####Forma Manual (MySQLDUMP)




Vamos a aprovechar las 2 máquinas que ya teniamos. 

Vamos a crear la base datos en MySQL. Para ello entramos en con el siguiente comando:

`mysql -uroot -p' 
y creamos la base de datos en las dos máquinas

```create database nombre;```

[!C1]

he introducido varios registros. 3 para ser exactos.


Vamos a usar el `mysqldump` para replicar la base de datos. Para ello entramos en mysql y "bloqueamos" las tablas con:

```mysql> FLUSH TABLES WITH READ LOCK;``` para evitar que se actualicen los datos mientras se está haciendo la réplica.

ejecutamos el comando `mysqldump contactos -u root -p > /home/andres/a.sql`


[!C2]

Ahora vamos a la máquina slave y ejecutamos:

`sudo scp andres@192.168.1.116:/home/andres/a.sql /home/andres`

[!C3]

Ya hemos copiado el archivo. Ahora solo falta replicarlo. 

Para ello:

```
mysql -u root –p
mysql> CREATE DATABASE ‘copiacontactos’;
mysql> quit
```

`mysql -u root -p copiacontactos < /home/andres/a.sql`

entramos para comprobar que todo esta correcto y:

[!C5]

Hemos replicado la base de datos de forma manual. Vamos a automatizar el proceso.

-------------------------
####Forma Automática (MySQLDUMP)

Vamos a comenzar accediendo a `/etc/mysql/my.cnf`

Una vez ahí, hacemos las siguientes modificaciones
* Comentamos `#bind-address 127.0.0.1`
* Indicamos la ruta para los errores `log_error = /var/log/mysql/error.log`
* Establecemos el identificador del servidor como server-id = 1

Guardamos el documento y reiniciamos

`/etc/init.d/mysql restart`

[!C6]

vamos a realizar las mismas configuraciones en el esclavo pero cambiando una cosa. Simplemente en `server-id = 2`

Reiniciamos y...

[!C7]


Vamos a la máquina master para configurar el acceso del esclavo. 


Y vamos a hacer lo siguiente dentro de sql
```
  mysql> CREATE USER esclavo IDENTIFIED BY 'esclavo';
  mysql> GRANT REPLICATION SLAVE ON *.* TO 'esclavo'@'%' IDENTIFIED BY 'esclavo';
  mysql> FLUSH PRIVILEGES;
  mysql> FLUSH TABLES;
  mysql> FLUSH TABLES WITH READ LOCK;
  mysql> SHOW MASTER STATUS;
```

[!C9]


Por últmo vamos al esclavo a probar suerte....

```
mysql> 
CHANGE MASTER TO MASTER_HOST='192.168.1.116', 
MASTER_USER='esclavo', MASTER_PASSWORD='esclavo', 
MASTER_LOG_FILE='mysql
-
bin.000001', MASTER_LOG_POS=501, 
MASTER_PORT=3306;

```














