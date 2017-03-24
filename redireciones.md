# Redirecciones

Algunas ocasiones debemos de poder ser capaces de redirigir, peticiones o resultados a otras páginas para ello Spring nos brinda un mecanismo bastante simple para poder hacer dicha acción utilizaremos el controlador de los ejemplos anteriores para ello en el controlador agregaremos el siguiente código:

```java
    @GetMapping("/")
    public String redirect(){
        return "redirect:/form/verform";
    }
```

Es un método simple usamos la anotación GetMapping, en el método que se crea pasamos un string con la palabra clave "redirect:" junto con el path al cual queremos que se direccione. para este caso usamos el path raíz o sea al ejecutar el código y colocamos en el navegador la ruta

> [http://localhost:8080/](http://localhost:8080/) o [http://localhost:8080/form/](http://localhost:8080/form/)

Automáticamente se redirección a la página de la vista del formulario. Ahora podemos usar otra forma la cual se usa un método RedirectView para hacer el retorno de la vista y que realice el redirecionamiento para esto definimos el siguiente código

```java
@GetMapping("/")
    public RedirectView redirect(){
        return new RedirectView("/form/verform");
    }
```

Al ingresar la url como en el ejemplo anterior, tendremos una funcionalidad similar a el código anterior

