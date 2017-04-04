# Spring Rest

Como todo set de herramientas que permiten el facilitar componentes para mejorar y adaptar a arquitecturas diferentes y que hoy en día son las que más se utilizan para el poder gestionar los datos en formato JSON, Spring permite el uso de Rest que se adapta a la gestión de la información orientándolas a la implementación de apis y acceso por url amigables, y así poder crear aplicaciones escalables y modulares donde el backend sea gestionado por nuestra app y las vistas sean consumidos por cualquier tecnología que permita el consumo de las respuestas para ellos podemos optar por Angular que es uno de los frameworks de creación de aplicaciones del lado del frontend.

para el uso de Rest en Spring solamente debemos anotar nuestras clases con el identificador **@RestController**, la diferencia que tendrá contra un controlador normal de spring es que el resultado o la respuesta del método se enviara en formato JSON para que este sea consumido por el método que más convenga en el cliente.

Creamos un nuevo controlador con el siguiente código.

```java
package com.proyecto.controller;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

import com.proyecto.entity.Curso;

@org.springframework.web.bind.annotation.RestController
@RequestMapping("/rest")
public class RestController {

    @GetMapping("/simple")
    public ResponseEntity<String> testSimple(){
        return new ResponseEntity<String>("OK", HttpStatus.OK);
    }

    @GetMapping("/complex")
    public ResponseEntity<Curso> testComplex(){
        Curso curso = new Curso(2, "Curso Spring", "Curso básico de Spring", 100, 25);
        return new ResponseEntity<Curso>(curso, HttpStatus.OK);
    }
}
```

Ahora en el nevegardor podemos acceder al primer controlador usando la url

> [http://localhost:8080/rest/simple](http://localhost:8080/rest/simple)

Veremos el siguiente resultado en el navegador

![](/assets/Captura de pantalla 2017-04-03 a la%28s%29 18.21.06.png)

Para acceder ver el resultado del segundo método accedemos por medio de la url

> [http://localhost:8080/rest/complex](http://localhost:8080/rest/complex)

El resultado es el siguiente.

![](/assets/Captura de pantalla 2017-04-03 a la%28s%29 18.22.40.png)

Al usar un cliente Rest y accedemos a la misma url obtenemos un resultado similar, y esto nos demuestra el potencial de usar Rest como opción para el manejo de peticiones.

![](/assets/Captura de pantalla 2017-04-03 a la%28s%29 18.28.32.png)

Con esto podemos crear implementaciones del tipo API Rest, y así poder tener una gama más amplia para que nuestras aplicaciones puedan ser utilizadas no solo desde el navegador Web, sino además de dispositivos móviles u otro elemento que soporte el consumo de JSON.

