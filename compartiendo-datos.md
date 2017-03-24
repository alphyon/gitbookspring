# Pasando Datos desde la plantilla

## Datos simples

Para pasar datos desde nuestros controladores hacia las plantillas de html haremos uso de los elementos que nos brinda el motor de plantillas thymeleaf para ello creamos un archivo html con el siguiente marcado

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

Donde indicamos con un marcado especial el uso de un elemento que será el que se encargue de esperar a que se envié el dato desde el controlador y este es el atributo que agregamos a la etiqueta span.

> th:text="${nombre}"

Ahora cuando definamos dentro de nuestro controlador un parámetro que pasaremos este será el lugar donde se carga dicho valor, creamos un nuevo controlador con el siguiente código.

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

como hicimos con el ejemplo de llamar a la plantilla aquí vemos las diferentes formas que tenemos para poder pasar valores del controlador hacia la plantilla html.

En el primer método como retornamos una cadena, para poder pasar los datos usaremos el objeto Model de Spring para poder definir atributos que se agregan a la respuesta http que esté haciendo en ese momento el cliente, y con ello se retorna la plantilla más un modelo con datos que son los básicos par-valor, por ello usamos la propiedad addAttribute\(&lt;nombre parámetro&gt;,&lt;valor parámetro&gt;\).

En el segundo caso el objeto ModelAndView recibe por medio de una propiedad addObject un elemento que debe retornar junto con la plantilla cuando esta es solicitada por el cliente.

## Datos Complejos

Ahora si necesitamos pasar más de un valor hacia la plantilla lo que podemos hacer es enviar un objeto con los elementos que necesitamos mostrar en dicha plantilla para ello creamos una nueva plantilla con el siguiente marcado:

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

Ahora creamos una nueva clase controller para hacer el paso del objeto hacia nuestra plantilla

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

Adicionalmente tenemos que crear un paquete con el nombre "**model**", y dentro del paquete creamos una clase con el siguiente código

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
    public Persona() {

    }



}
```

Con esto al ejeuctar nuestra app e ingresar al navegador por la url

> [http://localhost:8080/complex/datocomplex](http://localhost:8080/complex/datocomplex)

Podemos ver la siguiente información en pantalla

![](/assets/Captura de pantalla 2017-01-31 a las 16.15.42.png)

Esto significa que todo ha esta de forma correcta y que nuestros datos se estan pasando desde el controlador a la plantilla, pasados por medio de un objeto.

## Lista de datos

Cuando necesitamos pasar una lista de datos desde el controlador a la vista debemos realizar las siguientes codificaciones, primero creamos la clase en java para enviar el listado de personas

```java
package com.proyecto.controller;

import java.util.ArrayList;
import java.util.List;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

import com.proyecto.model.Persona;

@Controller
public class ListaController {
    @GetMapping("/lista")
    public String Personas(Model model) {
        model.addAttribute("personas", getPersonas());
        return "lista";
    }

    private List<Persona> getPersonas() {
        List<Persona> personas = new ArrayList<>();
        personas.add(new Persona("Juan", 33));
        personas.add(new Persona("Maria", 23));
        personas.add(new Persona("Diego", 32));
        personas.add(new Persona("Diego", 32));

        return personas;
    }
}
```

Lo que hemos hecho es crear un método que retorne una List de java en este caso con datos de personas que para nuestro ejemplo la realizamos de la manera más simple creando un objeto Lista y añadiendo los objetos correspondientes a la lista que vamos a pasar a la vista

en el método que envía los parámetros hacia la plantilla, en la propiedad **addAtributte**, en el valor del parámetro pasamos el llamado al método que retorna la lista en este caso **getPersonas\(\)** y listo con eso ya definimos que enviaremos un objeto con la lista de personas hacia la plantilla, en la cual debemos de agregar el siguiente marcado para poder recuperar los datos y mostrarlos en la pantalla.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8" />
<title>Lista de Personas</title>
</head>
<body>
    <table>
        <thead>
            <tr>
                <th>Nombre</th>
                <th>Edad</th>
            </tr>
        </thead>
        <tbody th:each="persona: ${personas}">
            <tr>
                <td th:text="${persona.nombre}"></td>
                <td th:text="${persona.edad}"></td>
            </tr>
        </tbody>
    </table>
</body>
</html>
```

Al ejecutar la aplicación obtendremos el siguiente resultado en el navegador.

![](/assets/Captura de pantalla 2017-01-31 a las 17.06.34.png)

Como podemos ver según los ejemplos realizados spring nos permite de una forma simple el poder pasar parámetros desde nuestros controladores hacia las vistas, solo debemos de tener en cuenta que datos vamos a enviar para poder crear los métodos y objetos necesarios para tenerlos a disposición de nuestras vistas.

