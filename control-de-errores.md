En esta parte vamos a revisar un punto adicional es una redirección especial, hacia páginas de errores, spring por defecto cuando no se encuentra un recurso nos muestra la siguiente página .![](/assets/Captura de pantalla 2017-03-07 a las 15.43.41.png)

Esta página de error la provee por defecto spring boot, y está configurada para que se muestre por defecto, recordemos estamos usando una versión base de tomcat, entonces esa no sería la página que veríamos al no encontrarse el recurso en el servidor.

![](/assets/Captura de pantalla 2017-03-07 a las 16.14.38.png)

Ahora tenemos la opción de crear una página personalizada, la cual será la que reemplazará a la página por defecto del spring boot, para ello solo debemos de crear una carpeta llamada **error** dentro de la carpeta **template** de nuestro proyecto, por defecto Spring verifica si esta carpeta y dentro de ella existe un archivo con nombre igual al código de error que se debe mostrar para este caso debe existir el archivo **404.html** en la siguiente imagen se muestra el ejemplo de una página personalizada

![](/assets/Captura de pantalla 2017-03-07 a las 16.34.08.png)

En este caso en nuestro proyecto debe estar con la siguiente estructura.

![](/assets/Captura de pantalla 2017-03-07 a las 17.22.03.png)

Como vemos ahí está la carpeta error y el código fuente del código del archivo 404.html, podemos hacer lo mismos con los errores 500, con solo agregar un archivo con el nombre 500.html, spring mostrara nuestra vista personalizada.

Realicemos un ejemplo de código que nos lance en error 500 en la respuesta creamos un controlador nuevo y agregamos el siguiente código

```java
package com.proyecto.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("showerror")
public class ErrorExampleController {
  @RequestMapping("show")
  public String errorLaunch(){
      int c = 4/0;

      return "Test" + c;
  }
}
```

vamos al navegador y escribimos la siguiente ruta

> [http://localhost:8080/showerror/show](http://localhost:8080/showerror/show)

obtendremos el siguiente resultado en la vista

![](/assets/Captura de pantalla 2017-03-07 a las 17.03.00.png)

En nuestro proyecto la estructura queda de la siguiente manera.

![](/assets/Captura de pantalla 2017-03-07 a las 17.18.43.png)

Ahora en este caso cuando se da un error, en el server se lanza una traza con los errores que se generan y por lo cual se lanza el error del tipo 500,

![](/assets/Captura de pantalla 2017-03-07 a las 17.24.42.png)

Podemos agregar un controlador para poder facilitar el manejo de dicha información a la hora que se genera algún tipo de excepción en el servidor para ello vamos a crear un nuevo controlador con el siguiente código.

```java
package com.proyecto.controller;

import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;

@ControllerAdvice
public class ErrorsController {
    public static final String VIEW_ERROR = "error/500";
    @ExceptionHandler(Exception.class)
    public String showInternalServerError(){
        return VIEW_ERROR;

    }
}
```

Al agregar este controlador y ejecutamos alguna página con error ya no se genera la traza por defecto en el servidor, esto es porque estamos definiendo que tendremos el control de esta información con el controlador que acabamos de crear.

## Logs

A la hora de ejecutar nuestros códigos es importante tener una bitácora de información que nos sirva para poder tener nociones de las ejecuciones de los procesos, advertencias, y muestra de errores con un nivel de información que pueda ser útil para los administradores de las aplicaciones, es por ello que es bueno agregar elementos que nos permitan lanzar estas trazas o logs para que al ejecutarse tengamos esa información de manera mas controlada y ordenada.

Vamos a realizar un ejemplo del uso de uso de log, estos logs se manejan a nivel de consola, no se guardan solo son información que quedan mientras el servidor esté funcionando, en nuestro controlador de ejemplo de manejo de los errores lo modificamos de esta forma.

```java
import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("showerror")
public class ErrorExampleController {
    private static final Log LOGGER = LogFactory.getLog(ErrorExampleController.class);
  @RequestMapping("show")
  public String errorLaunch(){
      LOGGER.info("INFO TRACE");
      LOGGER.warn("WARNING TRACE");
      LOGGER.error("ERROR TRACE");
      LOGGER.debug("DEBUG TRACE");
      int c = 4/0;

      return "Test" + c;
  }
}
```

Al ejecutar el servidor y hacemos la prueba ingresando a la url

> [http://localhost:8080/showerror/error/](http://localhost:8080/showerror/error/)

Obtendremos la página de error personalizada pero si vamos a la consola del servidor en ejecución vemos lo siguiente

![](/assets/Captura de pantalla 2017-03-08 a las 15.08.14.png)

En los logs podemos manejar diferentes tipos o niveles de logs que van desde nivel de información, o errores. Del desarrollo que hacemos depende el uso de cada nivel de uso de logs o la información que debemos mostrar.

