Como mencionamos en un punto anterior podemos sobrescribir algunas propiedades de las configuraciones, para ello podemos modificar el archivo application.properties que viene junto con nuestra estructura y para ello podemos hacer las configuraciones de acuerdo a propiedades ya definidas para saber cuáles son estas propiedades podemos ir a la siguiente url  [http://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html](http://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html), y ahí tenemos la documentación de las propiedades que podemos utilizar.

Entonces en nuestro archivo podemos configurar de acuerdo a como se muestra en la configuración.

ejemplo

```
spring.aop.auto=true 
spring.aop.proxy-target-class=false
```

Pero podemos crear un archivo con extensión ".yml" y en este caso se cambia el formato de cómo se escriben las propiedades, entonces para el caso de usar los archivos de configuración con la extensión antes mencionadas tendremos que poner el formato de la configuración como se muestra en el siguiente ejemplo.

```yaml
spring:
  aop:
    auto: true
    proxy-target-class: false
```

En este caso el formtao yml cambia en el sentido de dar estructura con tabulado a cada propiedad y puede ser mas entendible.

## Ejemplo de uso de configuración  app

Vamos a verificar como se aplican los cambios de configuración y para ello tomaremos una de las propiedades que están definidas en la página que mencionamos antes entonces cambiaremos la propiedad del banner que se carga cuando arrancamos la aplicación de spring, cual es el banner es el ascci que se muestra en la consola cuando se carga es lo que se muestra en la imagen.

![](/assets/Captura de pantalla 2017-01-30 a las 17.16.08.png)

Vamos a cambiar este mensaje por uno personalizado ahora como lo haremos, primero verificamos las propiedades

```
# BANNER
banner.charset=UTF-8 # Banner file encoding.
banner.location=classpath:banner.txt # Banner file location.
```

En este caso me dice que la localización del mensaje del banner se encuentra en la ruta base del proyecto y en el archivo banner.txt, ahora dicho archivo no se encuentra, entonces de donde carga el texto que vemos, ese se carga por defecto por las configuraciones que ya vienen precargadas en el módulo entonces para hacer el primer cambio creamos el archivo "banner.txt" en la carpeta de **resources** de nuestro proyecto y agregamos un mensaje cualquiera en este caso para probar ponemos el siguiente texto

```
*********************
Aplicacion de prueba
de Spring

Backend.app
*********************
```

Y arrancamos nuestra aplicación y en la pestaña de consola cambia el mensaje.

![](/assets/Captura de pantalla 2017-01-30 a las 17.23.13.png)

