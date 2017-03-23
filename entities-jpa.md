> ## Entities JPA.

Ahora que ya tenemos las configuraciones elaboradas vamos acrear una clase del tipo Entity que estara directamente relacionada con una tabla en el esquema de la base de datos que tendremos.

Primero creamos un paquete donde guadaremos nuestras entidades, entonces cramos el paquete **entity, **dentro del paquete creamos una clase con el siguiente codigo , en este caso la clase tiene una varaicion para poder ser utilizada con oracle.

```java
package com.proyecto.entity;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.SequenceGenerator;
import javax.persistence.Table;

@Entity
@Table(name="CURSOS")
public class Curso {

    @Id
    @GeneratedValue(generator="CURSOS")
    @SequenceGenerator(name="CURSOS", sequenceName="SEQ_CURSOS")
    @Column(name="ID")
    private int id;
    @Column(name="NOMBRE")
    private String nombre;
    @Column(name="DESCRIPCION")
    private String descripcion;
    @Column(name="PRECIO")
    private int precio;
    @Column(name="HORAS")
    private int horas;

    public Curso(){

    }

    public Curso(int id, String nombre, String descripcion, int precio, int horas) {
        super();
        this.id = id;
        this.nombre = nombre;
        this.descripcion = descripcion;
        this.precio = precio;
        this.horas = horas;
    }
    public int getId() {
        return id;
    }
    public void setId(int id) {
        this.id = id;
    }
    public String getNombre() {
        return nombre;
    }
    public void setNombre(String nombre) {
        this.nombre = nombre;
    }
    public String getDescripcion() {
        return descripcion;
    }
    public void setDescripcion(String descripcion) {
        this.descripcion = descripcion;
    }
    public int getPrecio() {
        return precio;
    }
    public void setPrecio(int precio) {
        this.precio = precio;
    }
    public int getHoras() {
        return horas;
    }
    public void setHoras(int horas) {
        this.horas = horas;
    }    
}
```

si queremos utilizarla con  mysql solo debemos de cambiar las configuraciones para la propiedad del id del codigo en este caso debe quedar de esta forma.

```java
@Id
@GeneratedValue
@Column(name="ID")
private int id;
```

Ahora si ejecutamos el servidor en sus trazas veremos, que se muestra una sentencia sql, esto es por que al usar Hibernate estmos utiizando un ORM, lo cual realiza un mapeo de una clase a un esquema de la base de datos entonces al hacer esto y por la configuracion

```yaml
hibernate:
    ddl-auto: create-drop
```

cada vez que se realize un despligue limpia y crea el esquema de la base de datos, al ejecutar el servidor en su traza podremos ver la creacion de los objetos de la base de datos segun las clases que tengamos anotadas como entidades JPA.

![](/assets/Captura de pantalla 2017-03-22 a las 18.37.26.png)

