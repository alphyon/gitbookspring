Como mencionamos en un punto anterior podemos sobreescribir algunas propiedades de las configutaciones, para ello pdemos modificaar el archivo  application.properties que viene junto con nuestra estructura y paara ello podemos hacer las configuraciones de acuerdo a propiedades ya definidas para saber cuales son estas propieades podemos ir a la sigueinte url http://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html, y ahi tenemos la documentacion de las proiedades que podemos utilizar 

entonces en nuestro archivo podemos configurar de acuerdo a como se muestra en la configuracion 

ejemplo 

```
spring.aop.auto=true 
spring.aop.proxy-target-class=false
```

pero podemos crear un archivo con extession ".yml" y en este caso se cambia el formato de como se escriben las pripiedades, entonces para el caso de usar los archivos de configuracion con la extension antes mencionadas tendremos que poner el formato de la configuracion como se muestra en el sigueinte ejemplo

```yml
spring:
  aop:
    auto: true
    proxy-target-class: false 
 
```

en este caso el formtao yml cambia en el sentido de dar estructura con sangrado a cada propiedad

## Ejemplo de uso de configuracion  app

vamos a verificar como se aplican los cambios de configuracion y para ello tomaremos una de las propiedades que estan definidads en la pagina que mencionamos antes 

entonces cambiaremos la prpiedad del banner que se carga cuando arrancamos la aplicacion de spring, cual es el banner es el ascci que se muestra en la consola cuando se carga es lo que se muestra en la imagen

![](/assets/Captura de pantalla 2017-01-30 a las 17.16.08.png) 

vamos a cambiar este mensaje por uno personalizado ahora como lo haremos

primero verificamos las propiedades 

```
# BANNER
banner.charset=UTF-8 # Banner file encoding.
banner.location=classpath:banner.txt # Banner file location.
```

en este caso me dice que la localizacion del mensaje del banner se encuentra el la ruta base del proyecto y en el archivo banner.txt, ahora dicho archivo no se encuentra, entonces de donde carga el texto que vemos, ese se carga por defecto por las configuraciones que ya vienen precargadas en el modulo entonces para hacer el primer cambio creamos el archivo "banner.txt" en la carpeta de **resources** de nuestro proyecto y agregamos un mensaje cualquiera en este caso para probar ponemos el siguiente texto

```
*********************
Aplicacion de prueba
de Spring

Backend.app
*********************
```



y arrancamos nuestra aplicacion y en la pestaña de consola cambia el mensaje

![](/assets/Captura de pantalla 2017-01-30 a las 17.23.13.png)





