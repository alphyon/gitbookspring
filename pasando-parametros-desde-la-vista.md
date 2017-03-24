Anteriormente hemos visto como se pasan datos desde el controlador hacia las vistas, ahora debemos saber cuál es la manera que podemos capturar datos que se envíen desde las vistas hacia el controlador, ya sea que se envían los datos por el request o por algún formulario para ello veremos cada uno de los métodos y las propiedades que podemos aplicar en spring para hacer las capturas de los datos y procesarlos en nuestros controladores

primero creamos nuestro controlador con el código siguiente:

```java
package com.proyecto.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.ModelAndView;

@Controller
@RequestMapping("/parametros")
public class RequestParamController {

        private static final String TEMPLATE_VIEW = "datos";

        @GetMapping("/request")
        public ModelAndView getRequest(@RequestParam(name="nombre") String nombreReq){
            ModelAndView mav = new ModelAndView(TEMPLATE_VIEW);
            mav.addObject("dato_nombre", nombreReq);
            return mav;
        }
}
```

Lo nuevo en este controlador es que hacemos uso de la anotación @RequestParam, que se encarga de capturar los datos que se envían desde el cliente para poder procesarlos, lo importante del uso de dicha anotación es el nombre del dato que se quiere capturar en este caso se identifica por la propiedad **name** de la anotación.

Ahora creamos el template para asociar a dicho código con el siguiente marcado:

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8" />
<title>Pasando datos</title>
</head>
<body>

<p>Datos enviado desde la vista:</p>
<p><span th:text="${dato_nombre}"></span></p>
</body>
</html>
```

Ejecutamos la aplicación y en el navegador pasamos la siguiente url

> [http://localhost:8080/parametros/request?nombre=Pedro](http://localhost:8080/parametros/request?nombre=Pedro)

Y en le navegador se muestra el siguiente resultado

![](/assets/Captura de pantalla 2017-01-31 a las 18.00.33.png)

Esto nos indica que se ha capturado el parámetro correspondiente y lo mostramos en pantalla ahora si en el navegador colocamos la url de esta forma

> [http://localhost:8080/parametros/request](http://localhost:8080/parametros/request)

Y la ejecutamos obtendremos el siguiente mensaje de error

![](/assets/Captura de pantalla 2017-02-01 a las 13.55.24.png)

En este caso el error surge porque la anotación **@RequestParam** por defecto se configura con el valor obligatorio es decir el parámetro se tiene que enviar, porque si no es así toma como un request erróneo por la falta de un parámetro, podemos modificar el comportamiento del método para evitar que se genere la página de error haciendo un cambio en la definición del método, lo que se hace es cambiar la propiedad **required** con valor false, nuestro método en el código debe quedar así.

```java
public ModelAndView getRequest(@RequestParam(name="nombre", required=false) String nombreReq)
```

Y si ejecutmaos la url como lo hicimos anteriormente obtendremos el siguiente resultado en el navegador.

![](/assets/Captura de pantalla 2017-02-01 a las 14.01.07.png)

Podemos agregar una propiedad adicional para mostrar un mensaje por defecto cuando no se pasa un dato modificamos el método en el código de la siguiente manera

```java
public ModelAndView getRequest(@RequestParam(name="nombre", 
                                required=false,defaultValue="Falta dato") String nombreReq)
```

y en el navegador obtenemos el siguiente resultado

![](/assets/Captura de pantalla 2017-02-01 a las 14.13.55.png)

Podemos usar otra forma para el paso de parámetros y esta es cuando la url es "amigable" o posee un formato como el siguiente tipo.

> [http://localhost:8080/parametro/request/juan](http://localhost:8080/parametro/request/juan)

En este caso el parámetro se envía de una forma un poco más legible para tratar este tipo de URL, Spring implementa una anotación la cual nos permite extraer el dato y procesarlo, para ello agregamos el siguiente código a la clase que creamos anteriormente para procesar los datos desde el cliente

```java
@GetMapping("/request2/{nombre}")
        public ModelAndView getRequest2(@PathVariable(name="nombre") String nombreReq ){
            ModelAndView mav = new ModelAndView(TEMPLATE_VIEW);
            mav.addObject("dato_nombre", nombreReq);
            return mav;
        }
```

Solo es un método adicional en el cual los cambios relevantes son los siguientes en la anotación **@GetMapping** pasamos un parámetro de la forma "/request2/{nombre}", esto es la convención que se usa para definir el mapeo como lo hemos comprobado en anteriores ejemplos pero hay un pequeño cambio hay una elemento encerrado entre llaves, esto va indicarle a spring que es un parámetro que pasaremos por la URL para que él lo pueda procesar usando la anotación **@PathVariable**, al hacer esto en nuestra clase y ejecutarla en el navegador por medio de la url

> [http://localhost:8080/parametros/request2/Jose](http://localhost:8080/parametros/request2/Jose)

obtenemos el resultado siguiente en el navegador

![](/assets/Captura de pantalla 2017-02-01 a las 15.08.20.png)

En la vista no hay un cambio significativo, pero en la lógica y en la estructura de como enviamos el dato si, con esto definido tenemos dos formas de como poder procesar request del tipo GET que nos envié el cliente.

