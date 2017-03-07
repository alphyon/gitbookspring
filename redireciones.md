# Redirecciones

Algunas ocasiones debemos de poder ser capaces de redirigir, peticiones o resultados a otras paginas  para ello Spring nos brinda un mecanismo bastane simple para poder hacer dicha accion utilizaremos el controlador de los ejemplos anteriores para ello en el controlador agregaremos el sigueinte codigo:

```java
    @GetMapping("/")
    public String redirect(){
        return "redirect:/form/verform";
    }
```

Es un metodo simple usamos la anotacion GetMapping, en el metodo que se crea pasamos un string con la palabra clave "redirect:" junto con el path al cual queremos que se direccione. para este caso usamos el path raiz o sea al ejecutar el codigo y colocamos en el navegador la ruta

> [http://localhost:8080/](http://localhost:8080/) o [http://localhost:8080/form/](http://localhost:8080/form/)

automaticamente se redirecciona a la pagina de la vista del formulario.

Ahora podemos usar otra forma la cual se usa un metodo RedirectView para hacer el retorno de la vista y que realize el redirecionamiento para esto definimos el sigueinte codigo

```java
@GetMapping("/")
    public RedirectView redirect(){
        return new RedirectView("/form/verform");
    }
```

Al ingresar la url como en el ejemplo anterior, tendremos una funcionalidad similar a el codigo anterior

En esta parte vamos a revisar un punto adicional es una redireccion especial, hacia paginas de errores, spring por defecto cuando no se encuentra un recurso nos muestra la sigueinte pagina

![](/assets/Captura de pantalla 2017-03-07 a las 15.43.41.png)

Esta pagina de error la provee por defecto spring boot,  y esta configurada para que se muestre por defecto, recordemos estamos usando una version base de tomcat, entonces esa no seria la pagina que veriamos al no encontrarse el recurso en el servidor.

![](/assets/Captura de pantalla 2017-03-07 a las 16.14.38.png)

Ahora tenemos la opcion de crear una pagina personalizada, la cual sera la que reemplazara a la pagina por defecto del spring boot, para ello solo debemos de crear una carpeta llamada **error** dentro de la carpeta **template** de nuestro proyecto, por defecto Spring verifica si esta carpeta y dentro de ella existe un archivo con nombre igual al codigo de error que se debe mostrar para este caso debe existir el archivo **404.html **en la siguiente imagen se muestra el ejemplo de una pagina personalizada

![](/assets/Captura de pantalla 2017-03-07 a las 16.34.08.png)

En este caso en nuestro proyecto debe estar con la siguiente estructura.

![](/assets/Captura de pantalla 2017-03-07 a las 17.22.03.png)

como vemos ahi esta la carpeta error y el codigo fuente del codigo del archivo 404.html, podemos hacer lo mismos con los errores 500, con solo agregar un archivo con el nombre 500.html, spring mostrara nuestra vista personalizada.

como podemos ver en la siguiente imagen

![](/assets/Captura de pantalla 2017-03-07 a las 17.03.00.png)

en nuestro proyecto la estructura queda de la siguiente manera.

![](/assets/Captura de pantalla 2017-03-07 a las 17.18.43.png)

Ahora en este caso cuando se da un error, en el server se lanza una traza con los errores que se generan y por lo cual se lanza el error del tipo 500,

![](/assets/Captura de pantalla 2017-03-07 a las 17.24.42.png)

podemos agregar un controlador para poder facilitar el manejo de dicha informacion a la hora que se genera algun tipo de excepcion en el servidor para ello vamos a crear un nuevo controlador con el siguiente codigo

```java
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





