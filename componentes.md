# Componentes Spring

Los componentes de Spring son elementos que permiten de una manera más practica el cargar objetos, esto es debido a que spring utiliza la inyección de dependencias de una forma muy sencilla ya que solo al hacer una anotacion **@componet**, nuestra clase se convierte en un elemento que se carga en memoria y estará a disposición de su utilización cuando este sea llamado, en otros objetos que lo necesiten.

Para crear un componente creamos una clase con el siguiente código, como un buena práctica los componentes se guardan en un paquete.

```java
package com.proyecto.components;

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

En este caso lo básico de dicho componentes es la anotación **@Component** y el nombre que damos a este ya que este sera lo que nos permite el aceder a el. Para usar este control creamos otra clase en nuestros controladores, hacemos el siguiente código

```java
package com.proyecto.controller;

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

Lo nuevo de esta clase es la parte donde hacemos el llamado a nuestro componente para ello usamos dos anotaciones una es **@autowired** su función es buscar beans\(que en este caso son los componentes\) para enlazarlos a la clase que estamos usando, y la anotación **@Qualifier** permite el definir específicamente que componente vamos a utilizar, esta anotación se usa para evitar errores por alguna ambigüedad en el nombre de algunos de los componentes.

Creamos una vista solo para realizar una prueba de la carga del componente, en nuestra carpeta de **templates** creamos el archivo **componente.html** y agregamos el siguiente marcado

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

Al ejecutar nuestro servidor y probar la url

> [http://localhost:8080/component/test](http://localhost:8080/component/test)

En la página solo nos carga el mensaje de la vista

![](/assets/Captura de pantalla 2017-03-10 a las 11.39.54.png)

Ahora en la consola de nuestro server podemos observar una salida

![](/assets/Captura de pantalla 2017-03-10 a las 11.40.08.png)Que es el mensaje que definimos en la clase que anotamos con @Component, que su función era escribir en el log un mensaje, lo importante en este caso es enfocarnos en la manera en como spring nos permite reutilizar elementos, se hace de una manera en la cual nos debemos más enfocar en nuestra lógica que en las configuraciones para poder utilizarlos.

Vamos a crear un componente más y lo vamos a asociar a una configuración que nos va ser útil para definir los tiempos de ejecución de un request, para ello creamos una clase con el siguiente código

```java
package com.proyecto.components;

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
    public boolean preHandle(HttpServletRequest request, 
    HttpServletResponse response, Object handler)
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

Este componente que creamos se encarga de que en cada petición que hagamos se dispare un método que se ejecuta antes que otros, y guardara el tiempo en milisegundos, que utilizaremos para calcular el tiempo total de la petición que lanzaremos con el segundo método que se ejecuta después que se complete la petición que lanzamos

Pero para que esto se ejecute y funcione como esperamos debemos de hacer un paso adicional y es crear un interceptor que será el encargado de que se lance la ejecución de los métodos anteriores.

Creamos un paquete llamado **configuration** para crear una clase de configuración con el siguiente código

```java
package com.proyecto.configuration;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurerAdapter;

import com.proyecto.components.RequestTimeComponent;

@Configuration
public class WebMvcConfiguration extends WebMvcConfigurerAdapter {

    @Autowired
    @Qualifier("requestTime")
    private RequestTimeComponent requestTimer;
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(requestTimer);
    }

}
```

Cada vez que hagamos una petición al servidor podemos ver en las trazas, por el componente que creamos el tiempo que se ejecuta cada petición

![](/assets/Captura de pantalla 2017-03-10 a las 15.57.25.png)Esto es gracias a el componente creado y la clase de configuración que se crea.

