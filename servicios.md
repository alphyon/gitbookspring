# Servicios

Un servicio es algo que podriamos definir como un elemento dentro de nuestros aplicaciones que setisfacen o complementan funcionalidades que no son al 100% parte de la logica del negocio, pero que son necesarias para que se cumpla el objetivo por el cual se esta desarrollando la aplicacion, estos elemetos podriamos identificarlos como los que nos sirven para hacer la gestion contra una base de datos, crear archivos, gestion de logs, servicios web u otros.

Vamos a crear un servicio, el cual a una clase que ya hemos creado lo pasara la informacion que esta debe de pasar a la vista, reutilizaremos la clase del controlador de la Lista de personas que creamos en la seccion [Compartiendo datos](/compartiendo-datos.md) en el apartado **lista de datos.**

Este es el codigo que modificaremos

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



