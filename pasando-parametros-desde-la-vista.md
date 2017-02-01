Anteriormente hemos visto como se pasan datos desde el controlador hacia las vistas, ahora debemos saber cual es la manera que podemos capturar datos que se envien desde las vistas hacia el controlador,  ya sea que se envian los datos por el request o por algun formulario para ello veremos cada uno de los metodos y las propiedades que podemos aplicar en spring para hacer las capturas de los datos y procesarlos en nuestros controladores

primero creamos nuestro controlador con el codigo siguiente:

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

Lo nuevo en este controaldor es que hacemos uso de la anotacion @RequestParam, que se encarga de capturar los datos que se envian desde el cliente para poder procesarlos, lo importante de el uso de dicha anotacion es el nombre del dato que se quiere capturar en este caso se identifica por la propiedad **name** de la anotacion.

Ahora creamos el template para asociar a dicho codigo con el siguiente marcado:

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

ejecutamos la aplicacion y en el navegador pasamos la sigueinte url

> [http://localhost:8080/parametros/request?nombre=Pedro](http://localhost:8080/parametros/request?nombre=Pedro)

y en le navegador se muestra el siguiente resultado

![](/assets/Captura de pantalla 2017-01-31 a las 18.00.33.png)

esto nos indica que se ha capturado el parametro correspondiente y lo mostramos en pantalla

ahora si en el navegador colocamos la url de esta forma 

> http://localhost:8080/parametros/request

y la ejecutamos obtendremos el siguiente mensaje de error



![](/assets/Captura de pantalla 2017-02-01 a las 13.55.24.png)

en este caso el error surge por que la anotacion @RequestParam por defecto se configura con el valor obligatorio es decir el parametro se tiene que enviar, por que si no es asi toma como un request erroneo por la falta de un parametro, podemos modificar el comportamiento del metodo para evitar que se genere la pagina de error haciendo un cambio en la definicon del metodo,  lo que se hace es cambiar la propiedad required con valor false, nuestro metodo en el codigo debe quedar asi.

```java
public ModelAndView getRequest(@RequestParam(name="nombre", required=false) String nombreReq)
```

y si ejecutmaos la url como lo hicimos anteriormente obtendremos el siguiente resultado en el navegador.

![](/assets/Captura de pantalla 2017-02-01 a las 14.01.07.png)

pdemos agregar una propiedad adicional para mostrar un mensaje por defecto cuando no se pasa un dato modificamos el metodo en el codigo de la siguiente manera

```java
public ModelAndView getRequest(@RequestParam(name="nombre", 
                                required=false,defaultValue="Falta dato") String nombreReq)
```

y en el navegador obtenemos el siguiente resultado 

![](/assets/Captura de pantalla 2017-02-01 a las 14.13.55.png)



podemos usar otra forma para el paso de parametros y esta es cuando la url es "amigable"o posee un formato como el siguiente tipo

> http://localhost:8080/parametro/request/juan

en este caso el parametro se envia de una forma un poco mas legible para tratar este tipo de URL, Spring implementa una anotacion la cual nos permite extraer el dato y procesarlo 

para ello agregamos el siguiente codigo a la clase que creamos anteriormente para procesar los datos desde el cliente

```java
@GetMapping("/request2/{nombre}")
		public ModelAndView getRequest2(@PathVariable(name="nombre") String nombreReq ){
			ModelAndView mav = new ModelAndView(TEMPLATE_VIEW);
			mav.addObject("dato_nombre", nombreReq);
			return mav;
		}
```



solo es un metod adicional en el cual los cambiso reelevantes son los siguientes en la anotacion @GetMapping pasamos un parmetro de la forma "/request2/{nombre}", esto es la convencion que se usa para definir el mapeo como lo hemos comprobado en anteriores ejemplos pero hay un pequeño cambio hay una elemento encerrado entre llaves, esto va ndicarle a spring que es un parametro que pasaremos por la URL para que el lo pueda procesar usando la anotacion @PathVariable, al hacer esto en nuestra clase y ejeucartla en el navegador por medio de la url 

> http://localhost:8080/parametros/request2/Jose

obtenemos el resultado siguiente en el navegador

![](/assets/Captura de pantalla 2017-02-01 a las 15.08.20.png) 

en la vista no hay un cambio significativo pero en la logica y en la estructura de como enviamos el dato si, con esto definido tenemos dos formas de como poder procesar request del tipo GET que nos envie el cliente



