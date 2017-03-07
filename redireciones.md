# Redirecciones

Algunas ocasiones debemos de poder ser capaces de redirigir, peticiones o resultados a otras paginas  para ello Spring nos brinda un mecanismo bastane simple para poder hacer dicha accion utilizaremos el controlador de los ejemplos anteriores para ello en el controlador agregaremos el sigueinte codigo:

```java
    @GetMapping("/")
    public String redirect(){
        return "redirect:/form/verform";
    }
```

Es un metodo simple usamos la anotacion GetMapping, en el metodo que se crea pasamos un string con la palabra clave "redirect:" junto con el path al cual queremos que se direccione. para este caso usamos el path raiz o sea al ejecutar el codigo y colocamos en el navegador la ruta

> [http://localhost:8080/](http://localhost:8080/) o [http://localhost:8080/form/](http://localhost:8080/form/)

automaticamente se redirecciona a la pagina de la vista del formulario.

Ahora podemos usar otra forma la cual se usa un metodo RedirectView para hacer el retorno de la vista y que realize el redirecionamiento para esto definimos el sigueinte codigo

```java
@GetMapping("/")
    public RedirectView redirect(){
        return new RedirectView("/form/verform");
    }
```

Al ingresar la url como en el ejemplo anterior, tendremos una funcionalidad similar a el codigo anterior



