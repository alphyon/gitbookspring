Primero usamos la herramienta que nos provee Pivotal, para poder generar el esqueleto de la aplicacion basica que deseamos crear para ello ingresamos a la url [http://start.spring.io/](http://start.spring.io/), en la cual podremos elegir las diferentes configuraciones para nuestra aplicacion basica

![](/assets/Captura de pantalla 2017-01-30 a las 14.39.31.png)

Para nuestro proyecto lo configuramos con las siguientes propiedades

**Group**: com.proyecto

**Artifact**: backend

**Name**: backend

**Description**: &lt;pon lo que desees de la descripcion &gt;

**Packaging**: jar

**Java Version**: 1.8

**Languaje**: Java

Elegimos 2 dependencia basicas : Web y Thymeleaf

![](/assets/Captura de pantalla 2017-01-30 a las 14.43.26.png)

Generamos el proyecto, se descarga y lo descomprimimos y copiamos la carpeta hacia la carpeta del Workbench de eclipse, ya una vez en el workbench, importamos el proyecto para poder empezar a hacer el desarrollo en el IDE

![](/assets/Captura de pantalla 2017-01-30 a las 14.48.21.png)

Importando el proyecto

click derecho en el area de project explorer elegmos la opcion **import&gt;import**

![](/assets/Captura de pantalla 2017-01-30 a las 14.49.38.png)

En la vantana emergente elegimos la opcion **Maven** o la buscamos si no esta visible, desplegamos la opciones del tipo maven y elegimos **Existing Maven Projects**

![](/assets/Captura de pantalla 2017-01-30 a las 14.50.32.png)

Esperamos a que se configure el proyecto y veremos en nuestro explorador de proyectos una jerarquia como la que vemos a continuacion

![](/assets/Captura de pantalla 2017-01-30 a las 15.07.20.png)

Ya con el proyecto en nuestro IDE, tendremos que descargar las depenedencias que son las librerias necesarias para poder ejecutar nuestra aplicacin Spring Boot, podemos hacerla de dos formas la primer es dar clic derecho seobre el poyecto e ir a la opcion **Maven&gt;Update  Project**

![](/assets/Captura de pantalla 2017-01-30 a las 15.11.59.png)

O lo podemos hacer desde la terminal de nuestro equipo para ello vamos a la ubicacion donde esta el proyecto y ejecutamos el siguiente comando

**mvn clean install**

al ejecutar el cmando se descarnga las librerias necesarias y ejecuta los test que esten creados para nuestra aplicacion y adicionalmente arranca el servidor de pruebas.

con esta ejecucion inicial podemos ir al IDE y revisar ls cambios que se han realizado, o poder controlar el inicio o parada de nuestro server para ello vamos al IDE y vamos a la pestaña del Boot Dash y elegimos la opcion local y el nombre de nuesra app

![](/assets/Captura de pantalla 2017-01-30 a las 15.20.20.png)

con los controles de la parte superior de la pestaña podemos iniciar o para la ejecucuion de nuestra aplicacion.

al arrancar nuestra app en la pestaña de la consola debemos ver algo como se muestra en la imagen

![](/assets/Captura de pantalla 2017-01-30 a las 15.22.57.png)con estos pasos realizados ya podemos empezar a desarrollar la logica de nuestra aplicacion 

