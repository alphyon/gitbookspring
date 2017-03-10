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

> http://localhost:8080/component/test



