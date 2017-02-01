En la seccion anterior se revisaron las formas de procesar informacion por medio de request usando el metodo GET de HTTP esta es la forma habitual de pasar parametros desde el cliente por URL, en esta seccion haremos uso del metodo POST que se usa en los envios de datos desde formularios web.

Entonces vamos a trabajar con un ejemplo de envio y procesamiento basico de los datos para este caso usaremos 2 vistas una que captura los datos por un formulario y otra que se encarga de mostrar los datos que captura el controlador y los pasa para ser mostrados

creamos la platilla para el formulario

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8" />
<title>Formulario</title>
</head>
<body>
    <form action="#" th:action="@{/form/addpersona}" th:object="${persona}" method="post" >
        <p>Nombre<input type="text" th:field="*{nombre}" /></p>    
        <p>Edad<input type="number" th:field="*{edad}" /></p>    
        <p><input type="submit" value="enviar"  /></p>    
    </form>
</body>
</html>
```

sol es de aclarar que en este caso por usar el motor de plantillas thymeleaf, asociamos a componentes del mismo el action del formulario\(th:action\) y definimos un objeto que es el modelo de datos\(th:object\) que deseamos que sea procesado por el formulario, ahora en el template de la vista definimos el sigueinte marcado

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8" />
<title> Datos desde Formulario</title>
</head>
<body>
<h1><span th:text="${persona.nombre}"></span> -- <span th:text="${persona.edad}"></span></h1>
</body>
</html>
```

Ahora creamos el controlador 

