# Configuracion del proyecto con Hibernate.

Vamos a realizar las configuraciones básicas para poder usar conexiones a base de datos, usando JPA con Hibernate lo primero que tenemos que hacer es identificar, el motor de la base de datos que se va a utilizar para generar la persistencia de los datos. Vamos a realizar 2 tipos de configuraciones una enfocada a **mysql** y la otra a **Oracle 11g.**

Primero configuramos las dependencia para la gestion de los datos, en el pom.xml agregamos lo siguiente.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

Despues agregamos el driver segun el motor a la base de datos que nos conectáremos.

##### Para mysql

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```

##### Para Oracle

Primero debemos, hacer una configuración básica vamos a registrar el driver de Oracle en el repositorio local de maven, se debe descargar el driver, con la descarga realizada debemos ejecutar en una terminal el siguiente comando.

> mvn install:install-file -Dfile=ojdbc7.jar  -DgroupId=com.oracle -DartifactId=ojdbc7 -Dversion=12.1.0.1 -Dpackaging=jar

Con esta instrucción realizamos un repositorio local con el driver de Oracle para poder agregarlo a las dependencias del proyecto, ya con esto ejecutado la dependencia en el pom.xml debe quedar de esta forma

```xml
<dependency>
    <groupId>com.oracle</groupId>
    <artifactId>ojdbc7</artifactId>
    <version>12.1.0.1</version>
</dependency>
```

Ahora en el archivo **application.ym**l, agregamos las siguientes configuraciones.

##### para mysql

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mydb
    username: root
    password: root
  jpa:
    show-sql: true
    hibernate:
      ddl-auto: update
      naming:
        strategy: org.hibernate.cfg.ImprovedNamingStrategy
    properties:
      hibernate:
        dialect: org.hibernate.dialect.MySQL5Dialect
```

##### para Oracle

```yaml
spring:
  datasource:
    driver-class-name: oracle.jdbc.OracleDriver
    url: jdbc:oracle:thin:@//localhost:49161/xe
    username: springtest
    password: springtest
    dbcp:
      validation-query: SELECT 1 FROM dual
      test-on-borrow: true
  jpa:
    database-platform: org.hibernate.dialect.Oracle10gDialect
    show-sql: true
    hibernate:
      ddl-auto: create-drop
```

Según el gestor y las configuraciones del mismo debemos cambiar algunos valores \(username, password, dialect u otros\). Con estas configuraciones podemos tener listo nuestro entorno para la persistencia de los datos.

