# Configuracion del proyecto con Hibernate.

vamos a realizar las configuraciones basicas para poder usar conecciones a base de datos, usando JPA con Hibernate lo primero que tenemos que  hacer es identificar, el motor de la base de datos que se va a utilizar para generar la persistencia de los datos.

vamos a realizar 2 tipos de configuraciones una enfocada a mysql y la otra a oracle 11g.

primero configuramos las dependencia para la gestion de los datos, en el pom.xml agregamos lo siguiente.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

despues agregamos el driver segun el motor a la base de datos que nos conectaresmos.

##### para mysql

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```

##### para Oracle

primero debemos, hacer una configuracion basica vamos a registrar el driver de oracle en el repositorio local de maven , primero se debe descargar el driver, y con esto debemos ejecutar en una terminal el siguiente comando.

> mvn install:install-file -Dfile=ojdbc7.jar  -DgroupId=com.oracle -DartifactId=ojdbc7 -Dversion=12.1.0.1 -Dpackaging=jar

con este comando realizamos un repositorio local con el driver de oracle para poder agregarlo a las dependencias del proyecto, ya con esto ejecutado la dependencia en el pom.xml debe quedar de esta forma

```xml
<dependency>
    <groupId>com.oracle</groupId>
    <artifactId>ojdbc7</artifactId>
    <version>12.1.0.1</version>
</dependency>
```

Ahora en el archivo application.yml, agregamos las sigueintes configuraciones.

##### para mysql

```yml
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

##### para oracle

```yml
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



