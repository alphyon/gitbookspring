Componentes Spring

Los componentes de Spring son elelemntos que permiten de una manera mas practica el cargar objetos, esto es debido a que spring utiliza la inyeccion de dependencias de una forma muy sencilla ya que solo al hacer una anotacion @componet, nuestra clase se convierte en un elemento que se carga en memoria y estara a disposicion de su utilizacion cuando este sea llamado, en otros objetos que lo necesiten.

para crear un componente creamos una clase con el siguiente codigo, como un buena practica los componentes se guardan en un paquete.

```java
import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.stereotype.Component;

@Component("ejemploComponent")
public class EjemploComponent {
    private static final Log LOG = LogFactory.getLog(EjemploComponent.class); 
    public void showInLog(){
        LOG.info("Log desde componente");
    }

}
```

en este caso lo basico de dicho componentes es la anotacion @Component y el nombre que damos a este ya que este sera lo que nos permite el aceder a el.

para usar este control creamos otra clase en nuestros controladores, hacemos el sigueinte codigo

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import com.proyecto.components.EjemploComponent;

@Controller
@RequestMapping("/component")
public class ComponentController {

    @Autowired
    @Qualifier("ejemploComponent")
    private EjemploComponent ejemploComponente;

    @GetMapping("/test")
    public String show() {
        ejemploComponente.showInLog();
        return "componente";
    }
}
```

Lo nuevo de esta clase es la parte donde hacemos el llamado a nuestro componente para ello usamos dos anotaciones una es @autowired su funcion es buscar beans\(que en este caso son los componentes\) para enlazarlos a la clase que estamos usando, y la anotacion @Qualifier permite el definir especificamente que componente vamos a utilizar, esta anotacion se usa para evitar errores por alguna ambiguedad en el nombre  de algunos de los componentes.

cremos una vista solo para realizar una prueba de la carga del compnente, en nuestra carpeta de templates creamos el archivo componente.html y agregamos el siguiente marcado

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8" />
<title>Prueba Componente</title>
</head>
<body>
    <p>Test Componente -- ve las trazas en el server</p>
</body>
</html>
```

al ejecutar nuestro servidor y probar la url

> [http://localhost:8080/component/test](http://localhost:8080/component/test)

en la pagina solo nos carga el mensaje de la vista

![](/assets/Captura de pantalla 2017-03-10 a las 11.39.54.png)

Ahora en la consola de nuestro server podemos observar una salida

![](/assets/Captura de pantalla 2017-03-10 a las 11.40.08.png)que es el mensaje que definimos en la clase que anotamos con @Component, que su funcion era escribir en el log un mensaje, lo importante en este caso es enfocarnos en la manera en como spring nos permite reutilizar elementos, se hace de una manera en la cual nos debemos mas enfocar en nuestra logica que en las configuraciones para poder utilizarlos.

Vamos a crear un componente mas y lo vamos a asociar a una configuracion que nos va ser util para definir los tiempos de ejecucion de un request

para ello creamos una clase con el sigueinte codigo

```java
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.stereotype.Component;
import org.springframework.web.servlet.handler.HandlerInterceptorAdapter;

@Component("requestTime")
public class RequestTimeComponent extends HandlerInterceptorAdapter{

    private static final Log LOGGER = LogFactory.getLog(RequestTimeComponent.class);

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
            throws Exception {
        request.setAttribute("startTime", System.currentTimeMillis());
        return true;
    }

    @Override    
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, 
    Object handler, Exception ex)
            throws Exception {
        long startTime = (long)request.getAttribute("startTime");
        LOGGER.info("Request Time: " + (System.currentTimeMillis()-startTime) 
        + "ms URL de la peticion: " + request.getRequestURL().toString());
    }
}
```

esta componente que cremos se encarga de que en cada peticion que hagamos se dispare un metodo que se ejecuta antes que otros, y guardara el tiempo en milisegundos, que utilizaremos para calcular el tiempo total de la peticion que lanzaremos con el segundo metodo que se ejecutar despues que se complete la peticion que lanzamos

