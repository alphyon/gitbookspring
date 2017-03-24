Como primeros pasos creamos unos paquetes iniciales los cuales los usaremos para agrupar los componentes de nuestra aplicación según sea su funcionalidad para este caso se crearán tres paquetes los cuales se guardarán en el paquete principal **com.proyecto, **entonces creamos los tres paquetes con los siguientes nombres

* controller
* service
* repository

En nuestro IDE debe quedar como se muestra en la imagen

![](/assets/Captura de pantalla 2017-01-30 a las 16.39.00.png)

Explicando la estructura y las carpetas podemos identificar que dentro de un proyecto de spring tenemos lo siguiente

_**Spring Element**_

En esta parte se almacenan las referencias a los beans que se crean en nuestro proyecto.

_**src/main/java**_

Aquí se almacenan todas los archivos de clases que creamos para nuestro proyecto, se pueden agrupar en paquetes para una mejor organización.

_**src/main/resources**_

En esta ubicación se colocan todos los archivos estáticos de nuestra aplicación, podemos definir que toda imagen, archivos de estilos\(css\) o los archivos de JavaScript que se utilicen para nuestras aplicaciones web se agregan en esta ruta, si se usa un sistema de plantillas se deben colocar en esta ruta los archivos de dichas plantillas, adicionalmente se almacena el archivo **application.properties** que es el fichero donde se pueden sobrescribir parámetros de configuración de nuestras aplicaciones con Spring Boot

_**src/test/java**_

Aquí se colocan todos los test que se crean para hacer las pruebas automatizadas de nuestra app.

_**JRE System Library **_

Es el conjunto de librerías básicas del jdk para que se puedan ejecutar las aplicaciones.

_**Maven dependencies**_

Todas las librerías de terceros que utilicemos en nuestra app se almacenan en esta ubicación.

_**src **_

Muestra como esta ordenados en el sistema de archivos nuestros elementos del proyecto

