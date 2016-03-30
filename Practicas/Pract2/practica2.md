#*Práctica 2*

##*Objetivos de la práctica*
Los objetivos concretos de esta segunda práctica son:
* aprender a copiar archivos mediante ssh.
* clonar contenido entre máquinas.
* configurar el ssh para acceder a máquinas remotas sin contraseña.
* establecer tareas en cron.


##Aprender a copiar archivos mediante ssh.

Lo primero que deberemos hacer es conocer la IP de la máquina de la que queremos conectarnos vía ssh con el comando `ifconfig`. Una vez conocida la IP procederemos a conectarnos vía ssh, en donde creamos en la máquina de destino un .tar.gz. (debemos tener instalado previamente en la máquina que queramos conectarnos el servidor ssh `sudo apt-get instal openssh-server`)

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract2/capturas/tar.png" />
</p>

Verificamos en la otra máquina que se ha ejecutado correctamente el comando anterior.

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract2/capturas/lstar.png" />
</p>


##Clonar contenido entre máquinas.
Debemos comprobar que tenemos instalado la herramienta para clonar el contenido, rsync. De lo contrario deberemos instalarlo `sudo apt-get install rsync`.

Tras esto, deberemos modificar la configuración del ssh de ambas máquinas para poder conectarlos como usuarios **root**. El archivo a modificar que deberemos abrir con algún editor de texto se encuentra en: /etc/ssh/sshd_config (en mi caso utilicé nano).

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract2/capturas/rsync.png" />
</p>

En la línea "PermitRootLogin" deberá estar a "yes". Una vez que lo modifiquemos deberemos reiniciar el servicio ssh para que se aplique dicha configuración (comando: `service ssh restart`).

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract2/capturas/sshconfig.png" />
</p>

##Configurar el ssh para acceder a máquinas remotas sin contraseña.

Para poder conectarnos vía ssh sin contraseña, deberemos generar una clave(internamente se crea una clave pública y una clave privada) con el comando `ssh-keygen -t dsa` en la máquina a la que queremos conectarnos.

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract2/capturas/keygen.png" />
</p>

Tras esto, deberemos copiar la clave pública a la otra máquina con el comando `ssh-copy-id -i .ssh/id_dsa.pub root@192.168.29.131`

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract2/capturas/copykey.png" />
</p>

Ya podremos conectarnos vía ssh sin necesidad de meter ninguna clave.

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract2/capturas/final.png" />
</p>

##E1stablecer tareas en cron.

Por último para establecer una tarea con cron, deberemos modificar el archivo que deberemos abrirlo con algún editor(de nuevo utilice nano). Para abrir este fichero tan solo deberemos ejecutar el comando `crontab -e`, y ya solo queda añadir la última línea como indico en la foto.

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract2/capturas/cron.png" />
</p>


En esta imagen podemos ver, que dicho comando de rsync se ejecutará todos los días en todo momento.







