##Práctica 3. Práctica Balanceo de carga
###SWAP 2015/2016


Partimos de las dos máquinas que he desarrollado en la [Práctica 2](https://github.com/Maverick94/swap1516/tree/master/practicas/practica2) 

Vamos a instalar un balanceador de carga, por lo tanto, necesitamos una tercera máquina virtual. 

Vamos a crear a *m3*. Usaremos *Ubuntu Server 14.04* otra vez

[C1](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C1.png)


La cual, le realizamos la configuración de las interfaces igual que en la anterior práctica para que se vean entre sí. 

m3 ---> 192.168.1.110 

Tambien, antes de empezar, hay que desactivar el `crontab` de la máquina 2 para que no se realicen copias entre los servidores y a la hora de mandar peticiones, diferenciemos la máquina que está contestando. 

Una vez acabado los preparativos, procedemos con la práctica en sí.

Vamos a proceder a instalar el `nginx`. Puesto que estoy usando la versión *Ubuntu Server 14.04*, no hace falta meter la clave del repositorio. Esta ya viene incluida. Por lo que, `apt-get install nginx` y andando... 

[C2](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C2.png)


Bien, vamos a configurar el archivo `/etc/nginx/conf.d/default.conf` 
Por defecto, actua como servidor web. Nosotros vamos a vaciar el contenido y rellenarlo con lo siguiente:

[C3](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C3.png)

Reinciamos el servicio con `service nginx restart` ... 

Ahora, en cada máquina final, vamos a modificar el archivo `index.html` ubicado en `/var/www/html` y vamos a ponerle al final el index, algo distintivo para diferenciarlas.

[C4](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C4.png)


Voy a lanzar `curl 192.168.1.110` en una  máquina cliente 5 veces. 

[C5](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C5.png)
[C6](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C6.png)
[C7](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C7.png)
[C8](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C8.png)
[C9](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C9.png)

Como se puede observar, balancea correctamente por turnos (Round-Robin).

Ahora vamos a darle el doble de capacidad a la máquina 1 respecto a la máquina 2.

[C10](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C10.png)

Con esto, el balanceador va redirigir la mayoría de las peticiones a la máquina 1.

Voy  a lanzar 4 veces el comando `curl 192.168.1.110` de nuevo.

[C11](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C11.png)
[C12](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C12.png)
[C13](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C13.png)
[C14](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C14.png)

Como podemos observar, hay un claro dominio de la máquina 1 :) 

-------------------------

Vamos a configurar ahora el otro programa que se pide: `haproxy`

lo primero que hacemos es, parar el otro servicio . Los dos escuchan en el puerto 80. Por lo tanto, para que funcione uno, el otro no debe de estar en funcionamiento. Por lo tanto:

`service nginx stop`

y ahora:

`apt-get install haproxy`

modificamos el archivo que se encuentra `/etc/haproxy/haproxy.cfg`

y le ponemos la configuración básica:

[C15](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C15.png)

lanzamos 3 veces el comando `curl 192.168.1.106` que es la dirección del balanceador y tenemos que:

[C16](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C16.png)
[C17](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C17.png)
[C18](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C18.png)

Se balancea por round-robin.

Ahora, con la siguiente configuración, vamos a darle más peso a la máquina 1.

[C19](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C19.png)

lanzamos `curl 192.168.1.106` 3 veces:

[C20](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C20.png)
[C21](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C21.png)
[C22](https://github.com/Maverick94/swap1516/blob/master/practicas/practica3/imagenes/C22.png)

Vemos que ahora ya tiene más peso la máquina 1.

.
