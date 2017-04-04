# Spring Security

Dentro del contexto de la herramientas y componentes que nos provee spring se encuentra utilidades para aplicar seguridad de nuestras aplicaciones, para ello Spring nos brinda un módulo para facilitar estas tareas. Solo basta el poder incluir dicho modulo a nuestro proyecto y realizar algunas configuraciones para su correcto uso. Con el módulo de Spring security podemos, login de usuarios, niveles de acceso y/o roles para nuestra aplicación, restricción de métodos o clases, para ciertos usuarios, y otras funciones según la necesidad y/o funcionalidades de nuestra app.

Para configurar el modulo primero debemos agregarlo a nuestra app para ello en el archivo **pom.xml** agregamos las siguientes líneas.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

Primero creamos unas clases de Entidad para poder guardar y gestionar los usuarios y roles en nuestra aplicación. Creamos el primer archivo en el paquete **entity**, ha dicho archivo le agregamos el siguiente código.

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

Esta será la clase de entidad de usuarios de nuestra app dependiendo de las necesidades pueden agregarse los campos necesarios. ahora creamos la otra clase que será la que permita gestionar los roles de los usuarios en el desarrollo, agregamos el siguiente código.

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

Como podemos ver son clases anotadas de entidad que se enlazan por medio de relaciones para su gestión de roles asignados a los usuarios.

Ahora creamos un repositorio para la gestión de los usuarios. Creamos una interface en el paquete **repository** y agregamos el siguiente código

```java
package com.proyecto.repository;

import java.io.Serializable;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import com.proyecto.entity.User;

@Repository("userRepository")
public interface UserRepository extends JpaRepository<User, Serializable>{
	public User findByUsername(String username);
} 
```

Recordar que la creación del repositorio implementa la mayoría de los métodos de gestión de los datos, en este caso solo agregamos un método que nos servirá para la búsqueda especifica del usuario por el nombre.

Ahora para se debe crear el componente de servicio que nos ayudara a poder gestionar los elementos para poder armar la data de permisos y usuarios para poder usarlos en la app. para ello creamos en el paquete **service** y en agregamos el siguiente código.

```java
package com.proyecto.service;

import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

import com.proyecto.entity.UserRole;
import com.proyecto.repository.UserRepository;

@Service("userService")
public class UserService implements UserDetailsService{

	@Autowired
	@Qualifier("userRepository")
	private UserRepository userRepository;
	
	@Override
	public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
		com.proyecto.entity.User user = userRepository.findByUsername(username);
		List<GrantedAuthority> authorities = buildAuthorities(user.getUserRole());
		return buildUser(user, authorities);
	}
	
	private User buildUser(com.proyecto.entity.User user, List<GrantedAuthority> authorities){
		return new User(user.getUsername(), user.getPassword(), user.isEnabled(), 
		true, true, true, authorities);
	}
	
	private List<GrantedAuthority> buildAuthorities(Set<UserRole> userRoles){
		Set<GrantedAuthority> auths = new HashSet<GrantedAuthority>();
		for (UserRole userRole: userRoles) {
			auths.add(new SimpleGrantedAuthority(userRole.getRole()));
			
		}
		return new ArrayList<GrantedAuthority>(auths);
	}

}
```

De esta clase lo que debemos destacar es que se usa un llamado del objeto **User** que es una implementación del Core del paquete de seguridad, este nos da permite el uso de ciertas propiedades básicas de nuestro usuario como son el nombre y la clave, pero además implementa algunas propiedades para validar el estado del usuario, ahora con la lógica de la implementación rellenamos dichos datos con la información que necesitamos de nuestra entidad y los roles que esta posea, esto es clave en la gestión de los roles ya que debemos saber exactamente que usuario y sus roles se usan en la petición en este caso cuando se de el logueo.

Ahora debemos de crear un elemento para la configuración de las propiedades básicas para el realizar el login y que se realicen algunas gestiones automáticas para ello en el paquete **configuration** creamos un archivo con el siguiente código.

```java
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled=true)
public class SecurityConfiguration extends WebSecurityConfigurerAdapter{
	@Autowired
	@Qualifier("userService")
	private UserDetailsService userService; 
	@Autowired
	public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception{
		auth.userDetailsService(userService).passwordEncoder(new BCryptPasswordEncoder());
	}

	@Override
	protected void configure(HttpSecurity http) throws Exception {
		http.authorizeRequests()
			.antMatchers("/css/*","/imgs/*").permitAll()
			.anyRequest().authenticated()
			.and()
			.formLogin().loginPage("/login").loginProcessingUrl("/logincheck")
			.usernameParameter("username")
			.passwordParameter("password")
			.defaultSuccessUrl("/loginsuccess").permitAll()
			.and()
			.logout().logoutUrl("/logout").logoutSuccessUrl("/login?logout")
			.permitAll();
	}

} 
```

En dicha clase se configuran, las url de donde se enviaran las peticones para procesar el ingreso de los usuarios, la url para enviar la peticon de deslogueo, asi como los sitios que estan permitidos el acceso sin seguridad. Adicionalmente hay un metodo para encriptar el parametro de la clave que se envia.

Para ir finalizando creaamos el controlador con el siguiente codigo.

```java
package com.proyecto.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class LoginController {
	
	@RequestMapping("/login")
	public String ShowloginForm(Model model,
		@RequestParam(name="error",required=false) String error,
		@RequestParam(name="logout",required=false) String logout){
		model.addAttribute("error", error);
		model.addAttribute("logout", logout);
		return "login";
	}
	
	@GetMapping({"/loginsuccess","/"})
	public String loginCheck(){
		return "redirect:/curso/lista";
	}

}
```

Y para el control desde el navegador creamos el archivo de la vista con el nombre **login.html **con el siguiente marcado

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8" />
<title>Login</title>
</head>
<body>
	<div class="container">
		<form th:action="@{/logincheck}" method="post">
			<input type="hidden" name="${_csrf.parameterNme}"
				value="${_csrf.token}" />
			<h2>Autenticarse</h2>
			<labe>Usuario:</labe>
			<input type="text" id="username" name="username" />
			<labe>Clave:</labe>
			<input type="password" id="password" name="password" />
			<div th:if="${error != null}">Usuario o clave incorrectos</div>
			<div th:if="${logout != null}">Deslogueo OK!!!</div>
			<button type="submit">Login</button>
		</form>
	</div>
</body>
</html>
```

Este es el formulario para el acceso lo que debemos notar aquí que por el módulo de seguridad se maneja el uso de un token este se hace enviando un dato en el campo oculto que se define en el formulario.

Ya con todos los elementos arrancamos el servidor y hacemos las pruebas para validar que nuestro acceso sea correcto. primero lo que tenemos que comprobar es que al querer acceder a alguna de la url que tengamos mapeados, siempre nos re direcciona a la vista del login.

![](/assets/Captura de pantalla 2017-04-03 a la%28s%29 17.03.47.png)

Si esto es correcto ingresamos los datos que tengamos almacenados para un usuario y comprobamos el poder ingresar a nuestra app. si todo es correcto al loguearnos podremos ver la vista a la cual se re direcciona por defecto o sea la que configuramos en nuestro controlador.

Con los roles que gestionamos podemos limitar el acceso a ciertos accesos estos se pueden configurar ya sea que queramos limitar el acceso a una clase, método, servicio o repositorio.

Para ello solo basta con anotar el elemento con el siguiente código

```java
@PreAuthorize("hasRole('ROLE_ADMIN')")
```

Está anotación lo que valida es el rol que este asociado al usuario que se a logueado, y si este tiene el permiso adecuado puede acceder a el elemento al cual se está gestionando la petición, como parámetro le podemos pasar una serie de roles o algunas funciones que permitan el limitar a los usuarios de acuerdo a las reglas de acceso que los dueños de la app definan.

