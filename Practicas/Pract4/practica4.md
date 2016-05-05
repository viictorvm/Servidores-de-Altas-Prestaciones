# Práctica 4.  Comprobar el rendimiento de servidores web.

Para la realización de esta práctica he elaborado un script en bash para las diferentes ejecuciones
y agilizar el proceso de forma que no sea una labor pesada.

Como carga en cada prueba de rendimiento pediremos al servidor el archivo prueba.php que está
en ambos servidores, en donde cuyo archivo sólo contiene un método: phpinfo()


## Comprobar el rendimiento de servidores web con Apache Benchmark.

Como antes he mencionado, he utilizado el siguente script para el analisis del servidor web.

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract4/capturas/scriptApache.png" />
</p>

Podemos observar que en el primer bucle, ejecutamos el comando ab en donde el número
de peticiones son 3000 y el número de concurrencia es de 500. (cuyos números serán igual
  en toda la práctica).

En el segundo bucle tenemos la misma estructura pero obviamente tendremos que modificar la IP
  a la que queremos analizar.

Una vez acabado esto, tendremos que matar el proceso de HAproxy en nuestra granja web,
  y ejecutar nginx. (con los comandos que en las anteriores prácticas he comentado). Y solo quedaría ejecutar de nuevo el segundo bucle y cambiando el archivo de salida (en mi caso, nginxApache.txt).

  Despues de recopilar la información sobre los valores a procesar, obtenemos esta tabla:
  <p align="center">
      <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract4/capturas/tablaab.png" />
  </p>

   Y en forma de gráfica:

  <p align="center">
      <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract4/capturas/abresultados.png" />
  </p>

En esta gráfica podemos observar donde el haproxy es un claro vencedor. M1 en donde se refiere a una sola máquina (ningún balanceador) es sin duda el que menos ha tardado en pasar el test, pero muchas estas peticiones
no se procesaban por el servidor web tal y como pretendiamos.

Por otra parte observamos que nginx y haproxy están igualados en ámbitos de peticiones por segundos y peticiones fallidas, pero mirando la parte del tiempo tomado en dicho test marca la diferencia entre Nginx y haproxy, siendo haproxy un claro ganador tanto por tiempo, como peticiones fallidas y por peticiones por segundo.


## Comprobar el rendimiento con Siege.

Al igual que con Apache, también he realizado un script para comprobar el rendimiento con siege.

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract4/capturas/scriptsiege.png" />
</p>

Como podemos observar en la captura, ejecutamos siege durante 60 segundos (en las tres pruebas). Ya que la salida de siege
es bastante larga, como filtro utilizo el comando 'sed' con el cual he creado un patrón donde borra todas las
líneas que aparezca "HTTP", y guardo la salida en el archivo para luego analizarlo.

Al igual que con ab, deberemos matar el proceso de HAproxy y ejecutar nginx para poder ejecutar de nuevo el bucle
con nginx en lugar de HAproxy.

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract4/capturas/tablasiege.png" />
</p>

Y en forma de gráfica:

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract4/capturas/siegeresultados.png"/>
</p>


En este caso podemos apreciar que la máquina 1, ósea sin balanceador, es el que tiene mejor resultados seguido del Nginx y como peor servicio en este caso es HAproxy. Ya que la disponibilidad y como consiguiente las transacciones fallidas son constantes, deberemos fijarnos tanto en la parte de las transacciones más larga y en el tiempo que ha tardado en general en realizar dicha prueba.
