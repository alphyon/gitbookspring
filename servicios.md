# Servicios

Un servicio es algo que podríamos definir como un elemento dentro de nuestras aplicaciones, que satisfacen o complementan funcionalidades que no son al 100% parte de la lógica del negocio, pero que son necesarias para que se cumpla el objetivo por el cual se está desarrollando la aplicación, estos elementos podríamos identificarlos como los que nos sirven para hacer la gestión de una base de datos, crear archivos, gestión de logs, servicios web u otros.

Vamos a crear un servicio, el cual a una clase que ya hemos creado lo pasara la información que esta debe de pasar a la vista, reutilizaremos la clase del controlador de la Lista de personas que creamos en la sección [Compartiendo datos](/compartiendo-datos.md) en el apartado **lista de datos.**

Este es el código que modificaremos

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

Primero vamos a crear paquetes, para mantener el orden de nuestras clases crearemos dos paquetes adicionales, uno con el nombre de **servicie, **dentro de este paquete creamos otro con el nombre **impl**, dentro del primer paquete creamos una interface con el siguiente código.

```java
package com.proyecto.service;

import java.util.List;

import com.proyecto.model.Persona;

public interface EjemploServicio {
    public abstract List<Persona> getListaPersonas();
}
```

En  el  paquete **impl** creamos una clase, con el código que implenta la inteface que creamos,  agregamos el siguiente código.

```
package com.proyecto.service.impl;

import java.util.ArrayList;
import java.util.List;

import org.springframework.stereotype.Service;

import com.proyecto.model.Persona;
import com.proyecto.service.EjemploServicio;

@Service("EjemploServicio")
public class EjmploServicioImp implements EjemploServicio {

    @Override
    public List<Persona> getListaPersonas() {
        List<Persona> personas = new ArrayList<>();
        personas.add(new Persona("Juan", 33));
        personas.add(new Persona("Maria", 23));
        personas.add(new Persona("Diego", 32));
        personas.add(new Persona("Diego", 32));

        return personas;
    }
}
```

En esta clase usamos la anotación **@service**, que es una anotación especial para identificar los beans que se usaran como servicios, aunque estos elementos son componentes de spring, pero se anotan de esta manera por semántica y en casos que sea necesario identificar exactamente la especificación de dichos componentes.

Aquí solamente lo que hacemos es meter una lista de datos, solo es un ejemplo base, pero podríamos poner una lógica para extraer los datos de una base de datos.

Ahora para usar el servicio, modificamos la clase que tenemos para mostrar la lista de personas en la vista con el siguiente código.

```java
package com.proyecto.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

import com.proyecto.service.EjemploServicio;

@Controller
public class ListaController {

    @Autowired
    @Qualifier("EjemploServicio")
    private EjemploServicio ejemploServicio;

    @GetMapping("/lista")
    public String Personas(Model model) {
        model.addAttribute("personas", ejemploServicio.getListaPersonas());
        return "lista";
    }    
}
```

Al ejecutar la url

> [http://localhost:8080/lista](http://localhost:8080/lista)

Obtendremos la lista de las personas pero en este caso la obtenemos desde el servicio y no como estaba declarada antes en el mismo controlador.

![](/assets/Captura de pantalla 2017-03-10 a las 18.25.20.png)

