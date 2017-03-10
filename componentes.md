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



