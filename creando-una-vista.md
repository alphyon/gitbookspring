para crear una vista primero creamos una platilla, en la carpeta **resources/templates** creamos un nuevo archivo html con el nombre hola mundo

con el siguiente marcado

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

y en creamos el controlador para poder lanzar esta vista al navegador cuando se haga la corespondiente llamda http

en la carpeta java/main/controller/ creamos el archivo **HolaMundoController.java **y agregamos el siguiente codigo

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

ponemos en ejecucion nuestra app y al teminar de cargar vamos al navegado e introducimos la sigueinte url

> [http://localhost:8080/say/holamundo](http://localhost:8080/say/holamundo)

entonces debemos ver lo siguiente en el navegador

![](/assets/Captura de pantalla 2017-01-30 a las 18.02.37.png)

En Spring podemos utilizar formas diferentes para poder cargar una plantilla y acceder a ella por una peticion http, para ello revisemos el siguente codigo:

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

con la anotacion @RequestMapping se define el acceso a las clases y a metodos Spring, en este caso se mapea el acceso o la forma de peticion desde el navegador,

En el primer metodo podemos observar que hacemos el uso de @GetMapping que es una alternativa que se implemento desde la version 4.3, a el uso de @RequestMapping con el value y el metodo de la peticion, es una forma abreviada de usar la anotacion  si podemos observar en este caso podemos ver que devolvemos un string con el nombre de la platilla que queremosver en el navegador

En el segundo metodo se usa la anotacion de forma tradicional, pero podemos usar la forma abreviada, pero en este caso devolvemos un objeto ModelAndView que devuelve un objeto con la plantilla, ya que el parametro que se pasa es el nombre de la plantilla.

Con esto podemos identificar que tenemos diferentes maneras de acceder a las plantillas.

### Pasando Datos desde la plantilla

para psar datos desde nuestros controladores hacia las plantillas de html haremos uso de los elementos que nos brinda el motor de plantillas thymeleaf para ello creamos un archivo html con el siguiente marcado

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8" />
<title>Pasando valores</title>
</head>
<body>
    <h2>Hola <span th:text="${nombre}"></span> !!!!</h2>
</body>
</html>   
```

donde indicamos con un marcado especial el uso de un elemento que sera el que se encargue de esperar a que se envie el dato desde el controlador y este es el atributo que agregamos a la etiqueta span

> th:text="${nombre}"

Ahora cuando definamos dentro de nuestro controlador un parametro que pasaremos este sera el lugar donde se carga dicho valor

ahora creamos un nuevo controlador con el siguiente codigo

```java
package com.proyecto.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
@RequestMapping("/val")
public class PasarValoresController {
	public static final String VALORES_VIEW = "pasovalores";
	
	
	@GetMapping("/pasovalor") 
	public String valoresString(Model model){
		model.addAttribute("nombre", "Juan Perez");
		return VALORES_VIEW;
	}
	
	@GetMapping("/pasovalormav")
	public ModelAndView valoresMaV(){
		ModelAndView mav = new ModelAndView(VALORES_VIEW);
		mav.addObject("nombre", "Juan");
		return mav;
	}

}
```



como hicimos con el ejemplo de como llamar a la plantilla aqui vemos las diferentes formas que tenemos para poder pasar valores del controlador hacia la plantilla html

En el prime metodo como retornamos una cadena, para poder pasar los datos usaremos el objeto Model de Spring para poder definir attibitos que se agregan a la respuesta http que este haciendo en ese momento el cliente, y con ello se retorna la plantilla mas un modelo con datos que son los basicos par-valor, por ello usamos la propiedad addAttribute\(&lt;nombre parametro&gt;,&lt;valor parametro&gt;\).

En el segundo caso el objeto ModelAndView recibe por medio de una propiedad addObject un elelemnto que debe retornar junto con la plantilla cunado esta es solicitada por el cliente. 

