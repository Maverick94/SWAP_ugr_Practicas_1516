##Práctica 6. Discos en RAID

Configurar dos discos en RAID 1. Los discos se añadirán a un sistema ya instalado y funcionando, de forma que en total tendremos tres discos.

Vamos a instalar el software necesario


`sudo apt-get install mdadm`


vamos a crear el RAID 1 con el comando 

`sudo mdadm -C /dev/md0 --level=raid1 --raid -devices=2 /dev/sdb /dev/sdc`

[!C1]


Una vez creado, le damos formato

[!C2]


vamos a crear un directorio ahora con los comandos

`sudo mkdir /dat`
`sudo mount /dev/md0 /dat`

[!C3]


Para finalizar el proceso, conviene configurar el sistema para que monte el dispositivo RAID creado al arrancar el sistema.

[!C4]

y al final del `/etc/fstab` anotaremos lo siguiente:

[!C5]

Vamos a simular un fallo de disco y una retirada en caliente 

con los comandos :

`sudo mdadm --manage --set -faulty /dev/md0 /dev/sdb`

y 

`sudo mdadm --manage --remove /dev/md0 /dev/sdb`
respectivamente

[!C6]


Y vamos a añadir  tambien en caliente otro disco. 

`sudo mdadm --manage --add /dev/md0 /dev/sdb`


[!C7]

