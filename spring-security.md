# Spring Security

Dentro del contexto de la heramientas y componentes que nos provee spring se encuentra utilidades para aplicar seguridad de  nuestras aplicaciones, para ello Spring nos brinda un modulo para facilitar estas tareas.

Solo basta el poder incluir dicho modulo a nuestro prouyecto y relaizar algunas configuraciones para su correcto uso.

Con el modulo de Spring security podemos, login de usuarios, niveles de acceso y/o roles para nuestra aplicacion, restriccion  de metodos o clases, para ciertos usuarios, y otra funciones segun la necesidad y/o funcionalidades de nuestra app.

Para configurar el modulo primero debemos agregarlo a neustra app para ello en el archivo **pom.xml **agregamos las siguientes lineas.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

  Actulizamos nuestro proyecto y creamos las sigueintes clases para poder hacer el login de usuarios a nuestra aplicacion.

primero creamos unas clases de Entidad para poder guardar y gestionar los usuarios y roles en nuestra aplicacion. Cremos el primer archivo en el paquete **entity. **a dicho archivo le agregamos el siguiente código.

```java
package com.proyecto.entity;

import java.util.HashSet;
import java.util.Set;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.FetchType;
import javax.persistence.Id;
import javax.persistence.OneToMany;
import javax.persistence.Table;

@Entity
@Table(name="usuario")
public class User {
	@Id
	@Column(name="username", unique=true, nullable=false,length=45)
	private String username;
	
	@Column(name="password", nullable= false, length=60)
	private String password;
	
	@Column(name="enabled", nullable=false)
	private boolean enabled;
	
	@OneToMany(fetch = FetchType.EAGER, mappedBy = "user")
	private Set<UserRole> userRole = new HashSet<UserRole>();
	
	public User(String username, String password, boolean enabled) {
		super();
		this.username = username;
		this.password = password;
		this.enabled = enabled;
	}
	public User(String username, String password, boolean enabled, Set<UserRole> userRole) {
		super();
		this.username = username;
		this.password = password;
		this.enabled = enabled;
		this.userRole = userRole;
	}
	public User(){}
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	public boolean isEnabled() {
		return enabled;
	}
	public void setEnabled(boolean enabled) {
		this.enabled = enabled;
	}
	public Set<UserRole> getUserRole() {
		return userRole;
	}
	public void setUserRole(Set<UserRole> userRole) {
		this.userRole = userRole;
	}	
} 
```

Esta sera la clase de entidad de usuarios de nuestra app dependiendo de las necesidades pueden agregarse los campos necesarios. ahora creamos la otra clase que sera la que permita gestionar los roles de los usuarios en el desarrollo, agegamos el sigueinte codigo.

```java
package com.proyecto.entity;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.FetchType;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.ManyToOne;
import javax.persistence.Table;
import javax.persistence.UniqueConstraint;

@Entity
@Table(name="rol_usuario",uniqueConstraints = @UniqueConstraint(
		columnNames = {"role","username"}))
public class UserRole {
	@Id
	@GeneratedValue
	@Column(name="user_role_id", unique=true, nullable=false)
	private Integer userRoleId;
	
	@ManyToOne(fetch = FetchType.LAZY)
	@JoinColumn(name = "username", nullable = false)
	private User user;
	
	@Column(name="role", nullable=false, length=45)
	private String role;
	public UserRole(User user, String role) {
		super();
		this.user = user;
		this.role = role;
	}
	
	public UserRole(){}

	public Integer getUserRoleId() {
		return userRoleId;
	}

	public void setUserRoleId(Integer userRoleId) {
		this.userRoleId = userRoleId;
	}

	public User getUser() {
		return user;
	}

	public void setUser(User user) {
		this.user = user;
	}

	public String getRole() {
		return role;
	}

	public void setRole(String role) {
		this.role = role;
	}	
}
```



