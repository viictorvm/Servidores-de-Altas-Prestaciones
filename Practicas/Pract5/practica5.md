# Práctica 5. Replicación de bases de datos MySQL

Los objetivos concretos de esta práctica son:
* Copiar archivos de copia de seguridad mediante ssh.
* Clonar manualmente BD entre máquinas.
* Configurar la estructura maestro-esclavo entre dos máquinas para realizar el
clonado automático de la información.


## Crear una BD e insertar datos

Primero tendremos que crearnos una base de datos, para ello (en mi caso en la máquina 1), ejecutaremos el comando
`mysql -uroot -p` e introduciremos la contraseña y seguidamente procederemos a crear dicha base de datos:


<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract5/capturas/creandobd.png" />
</p>

Como podemos observar en la captura, creamos la base de datos de nombre "swap", y creamos una tabla alumno con dos atributos.

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract5/capturas/bd2.png" />
</p>

Rellenamos la tabla, y comprobamos que todo se ha creado correctamente con el comando `select * from alumno`

## Replicar una BD MySQL con mysqldump

Para replicar una base de datos primero tendremos que evitar que la BD cambie nada hasta una vez replicado
por lo que ejecutaremos el comando ` FLUSH TABLES WITH READ LOCK` una vez ya dentro de la base de datos. Tras esto
con el comando `mysqldump swap -u root -p > /root/ejemplodb.sql` tal y como muestro en la captura.

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract5/capturas/copia.png" />
</p>

A dicha copia lo he nombrado como ejemplodb.ql, pero podemos ponerle el nombre que queramos. Una vez hecha la copia de la BD podremos deshacer el paso de evitar que la BD se actualice con el comando `UNLOCK TABLES;`

Tras replicar la BD deberemos copiarla a la máquina 2, en donde ejecutaremos el comando usado en la captura para copiarla:

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract5/capturas/scp.png"/>
</p>

Cabe decir que la IP que se muestra es la de nuestra máquina 1 (ya que está allí dicha réplica). La orden mysqldump no incluye en ese archivo la sentencia para crear la BD por lo que tendremos que crearla a mano en la máquina 2
mediante el comando ` CREATE DATABASE ‘ejemplodb’;` (para ejecutar el comando mencionado deberemos ejecutar anteriormente `mysql -u root –p`)


<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract5/capturas/exportarbd.png"/>
</p>

Y ahora si podemos exportar la base de datos sin problemas.

**NOTA:** Ambas base de datos deben de tener el mismo nombre, sino se actualizará como queremos.En este caso la base de datos debe llamarse "swap" y no "ejemplodb".

## Replicación de BD mediante una configuración maestro-esclavo

Lo primero deberemos revisar que en ambas máquinas (1 y 2) tenemos la misma versión de mysql. Con el comando `mysql --version` podremos verificarlo. Para ambas máquinas serán la misma configuración menos la variable "server-id".

La configuración que se aloja en /etc/mysql/my.cnf deberá:
1. Tener comentada la línea de " bind-address 127.0.0.1"
2. log_error = /var/log/mysql/error.log
3. log_bin = /var/log/mysql/bin.log

Y por último la variable "server-id", en la máquina 1 le indicaremos `server-id=1` y en la máquina 2 `server-id=2`.

Reiniciamos la base de datos con `/etc/init.d/mysql restart`.

En la máquina 1 que será el maestro:

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract5/capturas/maestro.png"/>
</p>

Una vez hecho esto, ejecutaremos el comando `SHOW MASTER STATUS;` que nos servirá para el próximo paso.

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract5/capturas/status.png"/>
</p>

Deberemos fijarnos en las variables para el comando a continuación: File y position.

En la máquina 2 una vez dentro del mysql ejecutaremos el comando ` CHANGE MASTER TO MASTER_HOST='192.168.29.131',
MASTER_USER='esclavo', MASTER_PASSWORD='esclavo', MASTER_LOG_FILE='bin.000002', MASTER_LOG_POS=474,
MASTER_PORT=3306;`

En donde MASTER_LOG_FILE se corresponde con el anterior "File" y  MASTER_LOG_POS con "Position". (la IP deberá ser de la M1).

Al finalizar dicho paso ejecutaremos los comandos ` UNLOCK TABLES;` y `SHOW SLAVE STATUS\G ` sucesivamente y nos saldrá una salida como en la captura.

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract5/capturas/final.png"/>
</p>

Si la variable “Seconds_Behind_Master” es distinto de “null”, todo funciona correctamente. [X]
