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



