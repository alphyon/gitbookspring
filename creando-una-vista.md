Para crear una vista primero creamos una platilla, en la carpeta **resources/templates** creamos un nuevo archivo html con el nombre hola mundo, con el siguiente marcado

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8" />
<title>Insert title here</title>
</head>
<body>
    <h1>Hola Mundo !!!!!</h1>
</body>
</html>
```

Y en creamos el controlador para poder lanzar esta vista al navegador cuando se haga la corespondiente llamda http en la carpeta java/main/controller/ creamos el archivo **HolaMundoController.java **y agregamos el siguiente código

```java
package com.proyecto.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/say")
public class HolaMundoController {
    @GetMapping("/holamundo")
    public String HolaMundo(){
        return "holamundo";
    }

}
```

Ponemos en ejecución nuestra app y al teminar de cargar vamos al navegado e introducimos la siguiente url

> [http://localhost:8080/say/holamundo](http://localhost:8080/say/holamundo)

Entonces debemos ver lo siguiente en el navegador

![](/assets/Captura de pantalla 2017-01-30 a las 18.02.37.png)

En Spring podemos utilizar formas diferentes para poder cargar una plantilla y acceder a ella por una petición http, para ello revisemos el siguiente código:

```java
package com.proyecto.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.servlet.ModelAndView;

@Controller
@RequestMapping("/example")
public class EjemploController {
    public static final String EJEMPLO_VIEW = "ejemplo";

    //primera forma
    @GetMapping("ejemploString") //forma alterna a @RequestMapping
    public String ejemploString(){
        return EJEMPLO_VIEW;
    }


    //segunda forma
    @RequestMapping(value="/ejemplomav", method = RequestMethod.GET)
    public ModelAndView ejemploMAV(){
        return new ModelAndView(EJEMPLO_VIEW);
    }
}
```

Con la anotación @RequestMapping se define el acceso a las clases y a métodos Spring, en este caso se mapea el acceso o la forma de petición desde el navegador, en el primer método podemos observar que hacemos el uso de @GetMapping que es una alternativa que se implementa desde la versión 4.3, a el uso de @RequestMapping con el value y el método de la petición, es una forma abreviada de usar la anotación si podemos observar en este caso podemos ver que devolvemos un string con el nombre de la platilla que queremos mostrar en el navegador.

En el segundo método se usa la anotación de forma tradicional, pero podemos usar la forma abreviada, pero en este caso devolvemos un objeto ModelAndView que devuelve un objeto con la plantilla, ya que el parámetro que se pasa es el nombre de la plantilla.

Con esto podemos identificar que tenemos diferentes maneras de acceder a las plantillas.

### 



