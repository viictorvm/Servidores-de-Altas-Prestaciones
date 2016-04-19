# Práctica 3. Balanceo de carga

Para la realización de esta práctica, tenemos que crear otra máquina virtual
en donde **NO** deberemos tener instalado Apache. El motivo es simplemente que
tanto el software de balanceo de carga como el Apache escuchan por el puerto 80,
por lo que habrá complicaciones en ese aspecto.

Una vez creado la tercera máquina virtual (necesariamente con Ubuntu), procedemos
a instalar el primer balanceador: Nginx.

***Nota:*** se recomienda instalar Ubuntu 15 ya que fuerza a instalar la última versión de
nginx y evitamos problemas de compatibilidad de versiones..etc (como en mi caso).

## Instalación y configuración de Nginx.

  Para instalar nginx simplemente deberemos ejecutar el comando `sudo apt-get install nginx`
.(podemos ver la versión del mismo con el comando `nginx -v`).

Una vez instalado, procedemos a modificar el archivo de configuración de nginx.
Para ello modificaremos el archivo: /etc/nginx/conf.d/default.conf

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract3/capturas/nginxconf.png" />
</p>

Deberemos sustituir la configuración por defecto por la configuración  indicada en la captura. Tendremos que cambiar la IP según la que tengamos
cada uno. Para ver la IP de cada máquina basta con ejecutar el comando `ifconfig`
en cada máquina.

Tras esto, reiniciamos nginx para que se actualicen los cambios con el comando
`sudo service nginx restart`. Seguidamente en este punto para comprobar que el balanceo
en teoría funciona deberemos ejecutar el comando `curl http://IPBALANCEO` en donde IPBALANCEO
se corresponde con la IP de esta nueva máquina que hemos creado.

**Nota:** Para ver que efectivamente balancea de una forma sencilla he modificado el index.html
de cada máquina.

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract3/capturas/comprobacion.png" />
</p>

En esta captura podemos ver como el balanceador envía las peticiones a una máquina y otra
alternando debido al algoritmo utilizado (por turnos o Round-robin).


## Instalación y configuración de HAproxy.

Una vez instalado nginx para proceder a la instalación de haproxy **deberemos** ejecutar el
comando `sudo service nginx stop` o desinstalar nginx(`sudo apt-get autoremove nginx`) debido a que ambos balanceadores
escuchan por el puerto 80.


Para instalar haproxy ejecutaremos el comando `sudo apt-get install haproxy`.
Al igual que nginx deberemos modificar la configuración por defecto que se encuentra
en: /etc/haproxy/haproxy.cfg

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract3/capturas/haproxyconf.png" />
</p>

Deberemos sustituirla por la configuración mostrada en la captura. Al igual que antes,
tenemos que modificar las IPs por las nuestras.

Una vez instalado y configurado correctamente ejecutaremos el comando `/usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg`, ***ojo*** deberemos ser root para poder ejecutarlo sin errores.

Para comprobar que efectivamente el balanceador funciona sin problemas ejecutaremos `curl http://IPBALANCEO` y la salida
de dicho comando será igual que al hacerlo con nginx.


## Instalación y configuración de Pound. (Opcional)

Lo primero que deberemos hacer es parar los servicios de nginx o haproxy ya que como comentamos antes habrá conflictos
a la hora de escuchar por el puerto 80. (`sudo service NombreServicio stop`)

Para instalar pound ejecutaremos el comando `sudo apt-get install pound`. La configuración será una bastante simple
de la documentación de [pound](https://help.ubuntu.com/community/Pound) . Deberemos modificar el archivo alojado en /etc/pound llamado pound.cfg tal y como mostramos a continuación.

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract3/capturas/poundconf.png" />
</p>

Como podemos ver en la captura la configuración se resume en la primera IP deberemos introducir la IP del balanceador y a continuación las dos IPs restantes igual que en los otros ejemplos. Siempre escuchará por el puerto 80.

Una vez hecho esto, en el archivo /etc/default/pound deberemos poner la variable "startup" a 1. Seguidamente solo falta reiniciar  dicho servicio (`sudo /etc/init.d/pound start`) y al ejecutar el comando para mandar las peticiones (`curl http://IPBALANCEO`) nos saldrá nuevamente la salida como en el primer ejemplo.
