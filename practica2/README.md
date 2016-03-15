##Práctica 2. Clonar la información de un sitio web
###Andrés Gallardo Molina


Lo primero que vamos a hacer es instalar *Ubuntu server 14.04* en dos máquinas virtuales. Yo he usado *VirtualBox*.

![Alt Text](https://github.com/Maverick94/swap1516/blob/master/practica2/imagenes/M1C1.png "Instalando las máquinas")

Una vez instaladas, nos logueamos en ellas:

![M1M2C1](https://github.com/Maverick94/swap1516/blob/master/practica2/imagenes/M1M2C1.png)

y vamos a ver si se ven entre ellas con el comando `ifconfig`


![M1M2C2](https://github.com/Maverick94/swap1516/blob/master/practica2/imagenes/M1M2C2.png)

Vemos que tienen la misma IP así que vamos a utilizar el material proporcionado por el profesor para arregarlo.

[Configurar red interna en VirtualBox](http://www.ccamposfuentes.es/2014/03/12/configurar-red-interna-virtualbox/)

Una vez arreglado el conflicto, vemos que , con la herramienta ping, mis máquinas ya se ven entre ellas


![C4](https://github.com/Maverick94/swap1516/blob/master/practica2/imagenes/C4.png)

donde máquina 1 tiene por IP: 192.168.1.100 y máquina 2 tiene por IP: 192.168.1.102


Ahora procedemos a la práctica en sí. En el directorio de la máquina 1 */var/www/* he creado una carpeta que se llama *pruebaCopia*

![C5]((https://github.com/Maverick94/swap1516/blob/master/practica2/imagenes/C5.png)

con el objetivo de comprobar que se va a copiar en la máquina 2 mediante la herramienta *rsync* ya que la opción de:

*tar czf - directorio | ssh equipodestino 'cat > ~/tar.tgz'*

sea demasiado ineficiente.

Para ello tecleo en la máquina 2 el comando:
*rsync -avz -e ssh root@machina1:/var/www/ /var/www/*
para copiar la carpeta *pruebaCopia* de la máquina 1 a la máquina 2


![C6](https://github.com/Maverick94/swap1516/blob/master/practica2/imagenes/C6.png)


Puesto que la máquina 1 va a ser el servidor principal y que vamos a automatizar el proceso con el crontab, necesitaremos un acceso sin contraseña para ssh desde la máquina 2. Si no, un administrador tendría que estar pendiente de la máquina 2 para teclear la contraseña cada vez que se quiera hacer una copia. Esto último no nos interesa, ya que perdería toda la eficacia.

Vamos a generar el acceso sin contraseña para ssh desde la máquina 2. Realmente, esto consiste en crear una clave compartida.

Usamos el comando *ssh-keygen -t dsa* para crearla.

![C7]((https://github.com/Maverick94/swap1516/blob/master/practica2/imagenes/C7.png)


Utilizando el comando *chmod 600 ~/.ssh/authorized_keys* le estaremos dando los permisos que necesita. Por defecto sale con esos permisos dados pero yo ejecuto el comando de nuevo por si acaso...

Ahora vamos a copiar la clave pública en la máquina 1 (la principal) 

Para ello usamos *ssh-copy-id -i .ssh/id_dsa.pub root@machina1* 
nos pedirá la contraseña del root de la máquina 1. Cuando se la demos, ya podremos acceder siempre que queramos desde la máquina 2 a la máquina 1 sin pedirle la contraseña **(y no al contrario)**

Hacemos la prueba.

![C8](https://github.com/Maverick94/swap1516/blob/master/practica2/imagenes/C8.png)

Por lo tanto, ya podemos automatizar el proceso de copia del contenido */var/www* desde la máquina 1 hacia la máquina 2 con *crontab*

Accedemos a crontab con el comando *nano /etc/crontab* y añadimos la línea que deseemos automatizar y con la frecuencia deseada.

![C9]((https://github.com/Maverick94/swap1516/blob/master/practica2/imagenes/C9.png)


Yo la he configurado para que se ejecute una copia de seguridad cada media hora todos los días del año infinitamente.

Por lo tanto, ya tenemos 2 máquinas haciendo de servidores funcionando donde la máquina 1 es la principal y va a contener el sitio web alojado y la máquina 2 va a estar realizando una copia identica de la máquina 1 cada media hora y estará preparada para actuar cuando la necesiten. 


#####Referencias:

[Configuración red interna en VirtualBox](http://www.ccamposfuentes.es/2014/03/12/configurar-red-interna-virtualbox/)

[Problemas para loguearme como root mediante ssh](http://askubuntu.com/questions/469143/how-to-enable-ssh-root-access-on-ubuntu-14-04)

[Configuración de la máquina para que, al hacer ping o conectarte por ssh, puedas llamarla por su nombre en vez de introducir su IP constantemente (ejemplo: *$ ping machina1* en vez de *$ping 192.168.1.100*)](http://ubuntuforums.org/showthread.php?t=1662246)