Primero usamos la herramienta que nos provee Pivotal, para poder generar el esqueleto de la aplicación básica que deseamos crear para ello ingresamos a la url [http://start.spring.io/](http://start.spring.io/), en la cual podremos elegir las diferentes configuraciones para nuestra aplicacion a desarrollar.

![](/assets/Captura de pantalla 2017-01-30 a las 14.39.31.png)

Para nuestro proyecto lo configuramos con las siguientes propiedades

**Group**: com.proyecto

**Artifact**: backend

**Name**: backend

**Description**: &lt; descripción &gt;

**Packaging**: jar

**Java Version**: 1.8

**Languaje**: Java

Elegimos 2 dependencia basicas : Web y Thymeleaf

![](/assets/Captura de pantalla 2017-01-30 a las 14.43.26.png)

Generamos el proyecto, se descarga y lo descomprimimos y copiamos la carpeta hacia la carpeta del Workbench de eclipse, ya una vez en el workbench, importamos el proyecto para poder empezar a hacer el desarrollo en el IDE

![](/assets/Captura de pantalla 2017-01-30 a las 14.48.21.png)

## Importando el proyecto

click derecho en el area de project explorer elegimos la opción **import&gt;import...**

![](/assets/Captura de pantalla 2017-01-30 a las 14.49.38.png)

En la vantana emergente elegimos la opcion **Maven** o la buscamos si no esta visible, desplegamos la opciones del tipo maven y elegimos **Existing Maven Projects**

![](/assets/Captura de pantalla 2017-01-30 a las 14.50.32.png)

Esperamos a que se configure el proyecto y veremos en nuestro explorador de proyectos una jerarquía como la que vemos a continuación

![](/assets/Captura de pantalla 2017-01-30 a las 15.07.20.png)

Ya con el proyecto en nuestro IDE, tendremos que descargar las dependencias que son las librerías necesarias para poder ejecutar nuestra aplicación Spring Boot, podemos hacerla de dos formas la primera es dar clic derecho sobre el proyecto e ir a la opción **Maven&gt;Update  Project**

![](/assets/Captura de pantalla 2017-01-30 a las 15.11.59.png)

O lo podemos hacer desde la terminal de nuestro equipo para ello vamos a la ubicación donde está el proyecto y ejecutamos el siguiente comando

> **mvn clean install**

Al ejecutar el comando se descargan las librerías necesarias y ejecuta los test que estén creados para nuestra aplicación y adicionalmente arranca el servidor de pruebas.

Con esta ejecución inicial podemos ir al IDE y revisar los cambios que se han realizado, o poder controlar el inicio o parada de nuestro server para ello vamos al IDE y vamos a la pestaña del Boot Dash y elegimos la opción local y el nombre de nuestra app

![](/assets/Captura de pantalla 2017-01-30 a las 15.20.20.png)

Con los controles de la parte superior de la pestaña podemos iniciar o para la ejecuión de nuestra aplicación. Al arrancar nuestra app en la pestaña de la consola debemos ver algo como se muestra en la imagen

![](/assets/Captura de pantalla 2017-01-30 a las 15.22.57.png)Con estos pasos realizados ya podemos empezar a desarrollar la lógica de nuestra aplicación

