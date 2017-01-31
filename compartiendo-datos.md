### Pasando Datos desde la plantilla

#### Datos simples 

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



#### Datos Complejos

Ahora si necesitamos pasar mas de un valor hacia la plantilla lo que podemos hacer es enviar un objeto con los elementos que necesitamos mostrar en dicha plantilla para ello creamos una nueva plantilla con el siguiente marcado:

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8" />
<title>Dato Complejo</title>
</head>
<body>
	<p>datos del objeto</p>
	<p>Nombre: <span th:text="${persona.nombre}"></span></p>
	<p>Edad: <span th:text="${persona.edad}"></span></p>
	
</body>
</html>
```

ahora creamos una nueva clase controller para hacer el paso del objeto hacia nuestra plantilla

```java
package com.proyecto.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

import com.proyecto.model.Persona;

@Controller
@RequestMapping("/complex")
public class ComplejoController {

	public static final String TEMPLATE = "complejo";
	@GetMapping("/datocomplex")
	public String datosComplejos(Model model){
		model.addAttribute("persona", new Persona("Juan", 25));
		return TEMPLATE;
	}
	
	@GetMapping("/datocomplexmav")
	public ModelAndView datosComplejosMav(){
		ModelAndView mav = new ModelAndView(TEMPLATE);
		mav.addObject("persona", new Persona("Maria", 30));
		return mav;
	}
}

```

adicionalmente tenemos que crear un paquete con el nombre "**model**", y dentro del paquete creamos una clase con el siguiente codigo

```java
package com.proyecto.model;

public class Persona {
	private String nombre;
	private int edad;

	public String getNombre() {
		return nombre;
	}

	public void setNombre(String nombre) {
		this.nombre = nombre;
	}

	public int getEdad() {
		return edad;
	}

	public void setEdad(int edad) {
		this.edad = edad;
	}

	public Persona(String nombre, int edad) {
		super();
		this.nombre = nombre;
		this.edad = edad;
	}

}
```

con esto al ejeuctar nuestra app e ingresar al mavegado por la url 

> http://localhost:8080/complex/datocomplex

podemos ver la siguiente informacion en pantalla

![](/assets/Captura de pantalla 2017-01-31 a las 16.15.42.png)

esto significa que todo ha esta de forma correcta y que nuestros datos se estan pasando desde el controlador a la plantilla, pasados por medio de un objeto.



