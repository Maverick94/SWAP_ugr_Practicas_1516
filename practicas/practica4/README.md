##Práctica 4. Comprobar el rendimiento de servidores web

El objetivo de esta práctica es medir el rendimiento de los servidores con dos software diferentes. Apache Benchmark y Siege

Teniendo las 3 máquinas creadas desde la práctica anterior, generamos una nueva que acturá de cliente. 

Una vez creada, voy a crear un archivo ejemplo html para todas las pruebas 

[!C1]
-------------------------
###Apache Benchmark


Vamos a probar el rendimiento con Apache Benchmark

tenemos que lanzar el comando:
`ab -n 1000 -c 10 http://192.168.1.102/example.html`

Lo haré 3 veces 10 iteraciones cada una. 1) Para el servidor sólo 2) para el servidor con nginx y 3) para el servidor con haproxy.

[TABLA]

[GRAFICA]


-------------------------

####Siege


Vamos a hacerlo ahora con siege.

Vamos a lanzar , como antes, 10 lanzamientos por cada tipo de servidor.

`siege -b -t60S -v http://direccion_de_cada_servidor/example.txt` 

y obtenemos los siguientes resultados:

[Tabla 2]

[Grafica]


