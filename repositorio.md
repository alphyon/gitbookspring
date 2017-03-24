Un repositorio en spring es un mecanismo  que encapsula, el almacenamiento, búsqueda y recuperación en una colección de objetos, esto es una implementación la cual  busca que las tareas repetitivas de mantenimiento en las colecciones de datos se realicen de una forma más fácil, por eso Spring Data proporciona el uso de una interfaz la cual implementa la mayoría de métodos  para realizar, las operaciones más comunes de mantenimiento en los datos y así el desarrollador se enfoque en la lógica del negocio y menos en dichas tareas.

Para crear nuestro repositorio, vamos a crear un paquete para mantener en orden nuestras clases creamos un paquete con nombre **repository**, en el creamos una nueva interface con el código siguiente.

```java
package com.proyecto.repository;

import java.io.Serializable;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import com.proyecto.entity.Curso;

@Repository("cursoRepo")
public interface CursoRepository extends JpaRepository<Curso, Serializable>{

}
```

Con esta interface, se implmentan las funcionalidades del repositorio, ya con esto podremos usar metodos para la gestion de la base de datos, ya hay varios predefinidos como se muestra en la imagen.

![](/assets/Captura de pantalla 2017-03-24 a las 14.36.28.png)

Ahora lo que debemos tomar en cuenta al crear el repositorio es que se usa la anotación **@Repository, **como en la mayoria de los ejemplos que hemos visto las anotaciones de este tipo indican que esta clase o interface se comparta como un bean y con esto podemos hacer de una forma mas facil las inyecciones de dependencia. Otro aspecto que hay que destacar que esta interface se hereda del objeto JpaRepository a esta se le pasan 2 objetos para que pueda trabajar uno es el tipo\(en este caso es la clase de entidad que se gestinaran los datos\) y el otro es la implementación de la clase serializable.

Ahora probaremos esta interface, crearemos un servico para que podamos implementar los métodos especificos y según sean necesarios para la gestion de datos de nuestra entidad Curos, ademas creamos un controlador con dos operaciones una que muestre los datos en la tabla de la base de datos y otro que permita agregar un nuevo registro, para poder realizar las operaciones y mostrar los resultados vamos a crar una unica vista.

Primero creamos el servicio hay que recodar que para esto se crea una inteface y la clase que implemente los metodos de dicha interface, este será el código de la interface que se guarda en el paquete **service**

```java
package com.proyecto.service;

import java.util.List;

import com.proyecto.entity.Curso;

public interface CursoService {
    public List<Curso> listAllCursos();
    public Curso addCurso(Curso curso);
    public int removeCurso(int id);
    public Curso updateCurso(Curso curso);
}
```

Y este sera el código que agragamos en la clase que creamos en el paquete **imp.**

```java
package com.proyecto.service.impl;

import java.util.List;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Service;


import com.proyecto.entity.Curso;
import com.proyecto.repository.CursoRepository;
import com.proyecto.service.CursoService;

@Service("cursoServiceImp")
public class CursoServiceImp implements CursoService {
    private static final Log LOG = LogFactory.getLog(CursoServiceImp.class);
    @Autowired
    @Qualifier("cursoRepo")
    private CursoRepository cursoRepository;

    @Override
    public List<Curso> listAllCursos() {
        LOG.info("Call listAllCursos()");
        return cursoRepository.findAll();
    }

    @Override
    public Curso addCurso(Curso curso) {
        LOG.info("Call addCurso()");
        return cursoRepository.save(curso);
    }

    @Override
    public int removeCurso(int id) {
        cursoRepository.delete(id);
        return 0;
    }

    @Override
    public Curso updateCurso(Curso curso) {
        cursoRepository.save(curso);
        return null;
    }

}
```

Estos seran los objetos que manejan la logica ahora para implementar y hacer uso de ellos cremos el siguiente controlador en el paquete **controller**.

```java
package com.proyecto.controller;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

import com.proyecto.entity.Curso;
import com.proyecto.service.CursoService;

@Controller
@RequestMapping("/curso")
public class CursoController {
    private static final String CURSOS_VIEW = "curso";
    private static final Log LOG = LogFactory.getLog(CursoController.class);
    @Autowired
    @Qualifier("cursoServiceImp")
    private CursoService cursoService;

    @GetMapping("/lista")
    public ModelAndView listAllCursos(){

        ModelAndView mav = new ModelAndView(CURSOS_VIEW);
        mav.addObject("cursos",cursoService.listAllCursos());
        mav.addObject("curso", new Curso());
        return mav;
    }

    @PostMapping("/agregar")
    public String addCurso(@ModelAttribute("curso") Curso curso){
        LOG.info("Call: addCurso() --- Param: " + curso.toString() );
        cursoService.addCurso(curso);
        return "redirect:/curso/lista";
    }
}
```

Y para finalizar creamos la vista con nombre **curso.html** y agregamos este marcado.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8" />
<title>Lista de Cursos</title>
</head>
<body>
    <table>
        <thead>
        <tr>
            <th>Nombre</th>
            <th>Descripción</th>
            <th>Precio</th>
            <th>Horas</th>
        </tr>
        </thead>
        <tbody>
        <tr th:each="curso : ${cursos}">
            <td><span th:text="${curso.nombre}"></span></td>
            <td><span th:text="${curso.descripcion}"></span></td>
            <td>$<span th:text="${curso.precio}"></span></td>
            <td><span th:text="${curso.horas}"></span></td>
        </tr>
        </tbody>
    </table>

    <form action="#" th:action="@{/curso/agregar}" th:object="${curso}" method="post">
        <p>Nombre: <input type="text" th:field="*{nombre}" /></p>
        <p>Descripción: <input type="text" th:field="*{descripcion}" /></p>
        <p>Precio: <input type="text" th:field="*{precio}" /></p>
        <p>Horas: <input type="text" th:field="*{horas}" /></p>
        <p><input type="submit" value="Guardar" /></p>
    </form>
</body>
</html>
```

Ahora ejecutamos el servidor y accedemos a la url

> http:/localhost:8080/curso/lista

![](/assets/Captura de pantalla 2017-03-24 a las 15.42.47.png)

Podemos ver una lista de cursos, estos han sido recuperados desde la tabla, ahora si rellenamos los datos que se solicitan en el formualrio y damos clic en el boton se debe guardar un nuevo regsitro en la base de datos. realizamos esa operación y el resultado se ve en pantalla

![](/assets/Captura de pantalla 2017-03-24 a las 15.46.41.png)

Tenemos un nueo registro de curso, en la traza del servidor podemos ver las siguientes salidas![](/assets/Captura de pantalla 2017-03-24 a las 15.48.12.png)

Podemos ver las salidas de log que hemos definido en las diferentes clases ademas por haber definio la propiedad sql-show y su valor en true podemos ver las sentencias de SQL cada vez que se hace una operacion de mantenimiento en la base de datos.

