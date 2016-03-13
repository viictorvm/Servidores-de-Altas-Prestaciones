#*Ejercicio de clase Tema 1*

## ***Apache***
Se caracteriza por la eficiencia en cuanto a contenidos dinámicos gracias a sus módulos(y por tanto más fácil de configurar). Gracias a la comunidad que hay por detrás, es un servicio con gran soporte y ayudas. 

## ***Nginx***
Se caracteriza por ser más eficiente para contenidos estáticos, está presente en empresas como  **Instagram**, **YouTube** y **GitHub**. Es más ligero que apache ya que reduce el porcentaje de memoria *RAM*, y cuenta con un balanceador de carga permitiendo una mayor escalabilidad.

## ***thttpd***
Se usa normalmente como reemplazo de varios servidores con múltiples funciones y adecuado para atender grandes volúmenes de datos estáticos. Por ejemplo servidor para el almacenamiento de imágenes.


## ***Cherokee***
Hasta 6 veces más rápido que Apache sirviendo contenido estático, 3 veces más rápido con contenido dinámico, se caracteriza por su facilidad de configuración. Cherokee puede ser incluso un balanceador de carga entre el servidor y otros servidores de aplicaciones, separando además funciones de servir contenido estático y dinámico entre servidores, aumentando extremadamente la eficiencia en la arquitectura.


## ***Node.js*** 
Node.js es especialmente adecuado para aplicaciones en las que desea mantener una conexión persistente desde el navegador, y de nuevo al servidor. Utilizando una técnica conocida como "long-polling", una aplicación que envía actualizaciones al usuario en tiempo real.
Esto significa que puede crear una aplicación de chat basado en navegador en Node.js que casi no requiere recursos del sistema para servir a un gran número de clientes.
