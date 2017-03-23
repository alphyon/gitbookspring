## Entities JPA.

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



