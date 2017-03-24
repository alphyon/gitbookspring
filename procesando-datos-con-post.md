En la sección anterior se revisaron las formas de procesar información por medio de request usando el método GET de HTTP esta es la forma habitual de pasar parámetros desde el cliente por URL, en esta sección haremos uso del método POST que se usa en los envíos de datos desde formularios web.

Entonces vamos a trabajar con un ejemplo de envió y procesamiento básico de los datos para este caso usaremos 2 vistas una que captura los datos por un formulario y otra que se encarga de mostrar los datos que captura el controlador y los pasa para ser mostrados creamos la platilla para el formulario.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8" />
<title>Formulario</title>
</head>
<body>
    <form action="#" th:action="@{/form/addpersona}" th:object="${persona}" method="post" >
        <p>Nombre<input type="text" th:field="*{nombre}" /></p>    
        <p>Edad<input type="number" th:field="*{edad}" /></p>    
        <p><input type="submit" value="enviar"  /></p>    
    </form>
</body>
</html>
```

Hay que  aclarar que en este caso por usar el motor de plantillas thymeleaf, asociamos a componentes del mismo el action del formulario\(th:action\) y definimos un objeto que es el modelo de datos\(th:object\) que deseamos que sea procesado por el formulario, ahora en el template de la vista definimos el siguiente marcado

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8" />
<title> Datos desde Formulario</title>
</head>
<body>
    <h1>
        <span th:text="${persona.nombre}"></span> -- 
        <span th:text="${persona.edad}"></span>
    </h1>
</body>
</html>
```

Ahora creamos el controlador que se encarga de manejar las dos vistas y de procesar los datos, creamos el archivo con el siguiente código

```java
package com.proyecto.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

import com.proyecto.model.Persona;

@Controller
@RequestMapping("/form")
public class FormController {

    public static final String FORM_VIEW = "form";
    public static final String RESULT_VIEW = "resultado";

    @RequestMapping("/verform")
    public String showForm(Model model){
        model.addAttribute("persona", new Persona());
        return FORM_VIEW;
    }

    @PostMapping("/addpersona")
    public ModelAndView addPersona(@ModelAttribute("persona") Persona personaForm){
        ModelAndView mav = new ModelAndView(RESULT_VIEW);
        mav.addObject("persona", personaForm);
        return mav;
    }

}
```

El primer método solo se encarga de servir al navegador la vista con el formulario, el segundo método captura los datos haciendo uso de la anotación **@ModelAttribute** y se la pasa a la vista del resultado, como podemos analizar es muy sencillo el trabajar con las anotaciones que nos brinda Spring para el poder procesar datos que se envían desde el cliente.

