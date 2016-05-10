# Práctica 6. Discos en RAID

Los objetivos concretos de esta práctica son:
* Configurar dos discos en RAID 1. Los discos se añadirán a un sistema ya
  instalado y funcionando, de forma que en total tendremos tres discos (el
  sistema operativo estará ya instalado en /dev/sda y añadiremos dos discos
  más, que serán el /dev/sdb y el /dev/sdc, para configurar el dispositivo de
  almacenamiento RAID en estos dos discos nuevos de igual tamaño).

* Hacer pruebas de retirar y añadir un disco “en caliente”, y comprobar que el
RAID sigue funcionando correctamente.

## Configuración del RAID por software

Lo primero que tendremos que hacer es añadir los dos discos para el RAID 1. Tendremos que tener la máquina apagada para añadir dichos discos.

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract6/capturas/discos.png" />
</p>

Una vez arrancada la máquina con el comando `sudo apt-get install mdadm` instalaremos el software necesario para la configuración.

El primer paso será ejecutar el comando `sudo mdadm -C /dev/md0 --level=raid1 --raid-devices=2 /dev/sdb /dev/sdc` que creará el RAID 1 y lo nombrará como "md0"  seguido de la ubicación de los dos discos que hemos añadido anteriormente.

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract6/capturas/creandoraid.png" />
</p>

Con el comando `sudo mkfs /dev/md0` daremos formato al RAID 1(ya que no hemos reiniciado la máquina). El formato dado será ext2. Ahora creamos el directorio que se montará en el RAID 1 con los comandos `sudo mkdir /dat &&
sudo mount /dev/md0 /dat` y tras esto, lo montaremos en el sistema con `sudo mount`.

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract6/capturas/correcto.png" />
</p>

Podemos ver en esta captura como todo se ha montado correctamente en la salida del comando `mount`.

Para comprobar que el RAID 1 se ha creado correctamente ejecutaremos el comando `sudo mdadm --detail /dev/md0`

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract6/capturas/detail.png" />
</p>

Ahora conviene configurar el sistema para que monte el dispositivo
RAID creado al arrancar el sistema.Ejecutamos el comando `ls -l /dev/disk/by-uuid/` donde nos quedaremos con el identificador del RAID 1
<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract6/capturas/id.png" />
</p>

Y modificaremos el archivo '/etc/fstab' de la siguiente forma: UUID=ccbbbbcc-dddd-eeee-ffff-aaabbbcccddd /dat ext2 defaults 0 0

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract6/capturas/config.png" />
</p>

Y ya tendremos el RAID 1 creado correctamente.

## Provocando fallos y retirar en "caliente".

Para simular un fallo en el RAID 1 ejecutaremos el comando `sudo mdadm --manage --set-faulty /dev/md0 /dev/sdb` (podemos ver el estado del RAID con el comando `sudo mdadm --detail /dev/md0`).

Lo retiraremos en "caliente" con el comando `sudo mdadm --manage --remove /dev/md0 /dev/sdb`

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract6/capturas/hotremoved.png" />
</p>

Y por último lo añadimos con el comando `sudo mdadm --manage --add /dev/md0 /dev/sdb`

<p align="center">
    <img src="https://github.com/viictorvm/Servidores-de-Altas-Prestaciones/blob/master/Practicas/Pract6/capturas/added.png" />
</p>
