#*Ejercicio T3.1*:

#Buscar con qué órdenes de terminal o herramientas gráficas podemos configurar bajo Windows y bajo Linux el enrutamiento del tráfico de un servidor para pasar el tráfico desde una subred a otra. #

Para windows podemos usar el comando  route (link:http://persoal.citius.usc.es/tf.pena/ASR/Tema_3html/node21.html).
Se caracteriza por algunas de las siguientes funcionalidades:  

1. [X] Permite modificar la tabla de routing, mostrando, añadiendo o borrando rutas
2. [X] Muestra las rutas definidas
3. [X] Permite añadir/borrar rutas estáticas
4. [X] Permite definir un gateway de salida por defecto para conectarnos al exterior
5. [X] Permite configurar el sistema para que actúe como un router


Para linux podemos utilizar el comando gufw( http://blog.desdelinux.net/como-configurar-el-firewall-en-ubuntu/) aunque linux tambien dispone del comando route al igual que windows


#*Ejercicio T3.2*:

# Buscar con qué órdenes de terminal o herramientas gráficas podemos configurar bajo Windows y bajo Linux el filtrado y bloqueo de paquetes. 

Bajo linux disponemos de herramientas como: iptables o route.
Para windows el comando route a parte del enrutamiento del tráfico de un servidor para pasar el
tráfico desde una subred a otra, también nos puede servir para el filtrado y bloqueo de paquetes. 

Herramientas gráficas destaca el más conocido tanto en ubuntu o en windows, **WireShark**.(http://manualwireshark.blogspot.com.es/)