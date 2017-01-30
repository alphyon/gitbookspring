Como primeros pasos creamos unos paquetes inciciales los cuales los usaremos para agrupar los componentes de nuestra aplicacion segun sea su funcionalidad para este caso se crearan tres paquetes los cuales se guardaran en el paquete principal com.proyecto

entonces creamos los tres paquetes con los siguientes nombres 

* controller
* service
* repository

en nuestro IDE debe quedar como se muestra en la imagen

![](/assets/Captura de pantalla 2017-01-30 a las 16.39.00.png)

Explicando la estrucura y las carpetas podemos identificar que dentro de un priyecto de spring tenemos lo siguiente

_**Spring Element**_

En esta parte se almacenan las referencias a los beans que se crean en nuestro poryecto

_**src/main/java**_

aqui se almacenan todas los archivos de clases que creamos para nuestro pryecto, se puedne agrupar en paquetes mpara una mejor organizacion

_**src/main/resources**_

en esta ubicacion se colocan todos los archivos estaticos de nuestra aplicacion, podemos definir que toda imagen, archivos de estilos\(css\) o los archivos de javascript que se utilizen para nuestras aplicaciones web se agregan en esta ruta, si se usa un sistema de plantillas aqui se deben colocar los archivos de dichas plantillas, adicionalmente aqui se almacena el archivo **application.properties **que es el fichero donde se pueden sobreescribir parametros de configuracion de nuestras aplicaciones con Spring Boot

_**src/test/java**_

aqui se colocan todos los test que se crean para hacer las pruebas unitarias de nuestra app

_**JRE System Library **_

es el conjunto de librerias basicas del jdk para que se puedan ejecutar las aplicaciones

_**Maven dependencies**_

todas las librerias de terceros que utilizemos en nuestra app se almacenana en esta ubicacion

_**src **_

muestra como esta ordenados en el sistema de archivos nuestros elementos del proyecto



