##Práctica 6. Discos en RAID

Configurar dos discos en RAID 1. Los discos se añadirán a un sistema ya instalado y funcionando, de forma que en total tendremos tres discos.

Vamos a instalar el software necesario


`sudo apt-get install mdadm`


vamos a crear el RAID 1 con el comando 

`sudo mdadm -C /dev/md0 --level=raid1 --raid -devices=2 /dev/sdb /dev/sdc`

[!C1](https://github.com/Maverick94/swap1516/blob/master/practicas/practica6/imagenes/C1.png)


Una vez creado, le damos formato

[!C2](https://github.com/Maverick94/swap1516/blob/master/practicas/practica6/imagenes/C2.png)


vamos a crear un directorio ahora con los comandos

`sudo mkdir /dat`
`sudo mount /dev/md0 /dat`

[!C3](https://github.com/Maverick94/swap1516/blob/master/practicas/practica6/imagenes/C3.png)


Para finalizar el proceso, conviene configurar el sistema para que monte el dispositivo RAID creado al arrancar el sistema.

[!C4](https://github.com/Maverick94/swap1516/blob/master/practicas/practica6/imagenes/C4.png)

y al final del `/etc/fstab` anotaremos lo siguiente:

[!C5](https://github.com/Maverick94/swap1516/blob/master/practicas/practica6/imagenes/C5.png)

Vamos a simular un fallo de disco y una retirada en caliente 

con los comandos :

`sudo mdadm --manage --set -faulty /dev/md0 /dev/sdb`

y 

`sudo mdadm --manage --remove /dev/md0 /dev/sdb`
respectivamente

[!C6](https://github.com/Maverick94/swap1516/blob/master/practicas/practica6/imagenes/C6.png)


Y vamos a añadir  tambien en caliente otro disco. 

`sudo mdadm --manage --add /dev/md0 /dev/sdb`


[!C7](https://github.com/Maverick94/swap1516/blob/master/practicas/practica6/imagenes/C7.png)

