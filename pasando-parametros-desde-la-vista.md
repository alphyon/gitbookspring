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

Lo nuevo en este controaldor es que hacemos uso de la anotacion @RequestParam, que se encarga de capturar los datos que se envian desde el cliente para poder procesarlos, lo importante de el uso de dicha anotacion es el nombre del dato que se quiere capturar en este caso se identifica por la propiedad name de la anotacion.

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

> http://localhost:8080/parametros/request?nombre=Pedro

y en le navegador se muestra el siguiente resultado

![](/assets/Captura de pantalla 2017-01-31 a las 18.00.33.png)

esto nos indica que se ha capturado el parametro correspondiente

