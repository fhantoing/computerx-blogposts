# Servidor de Configuraciones

Un Servidor de Configuraciones permite exteriorizar y centralizar la configuración de un proyecto de Microservicios o de un sistema distribuido en general.

En el caso de Spring Boot esta funcionalidad se implementa a través del Spring Cloud Config Server. Spring Cloud Config Server usa GIT como repositorio para los archivos de configuración y el control de versiones sobre los mismos.

Ventajas de Centralizar la configuración con GIT:

- Independencia con respecto al código fuente del proyecto.
- Mayor gestión y control de cambios sobre los archivos de configuración de un proyecto o sistema distribuido complejo.
- Uso de labels para referenciar configuraciones específicas.
- En un entorno de despliegue continuo facilita la integración automática entre los diversos entornos de tal forma que asegura que las aplicaciones tengan todo lo que necesitan para ejecutarse cuando migren de un entorno a otro.

## Configuración Servidor

Si esta utilizando Java 8 asegúrese se usar en su archivo POM.XML una versión compatible de spring-cloud con Java 8.
Nota: para Java 17 utilice versiones de spring cloud superiores a 2022.0.x.

```xml
<properties>
    <java.version>1.8</java.version>
    <spring-cloud.version>2020.0.3</spring-cloud.version>
</properties>
```

Adicione en su archivo POM.XML la dependencia de maven: spring-cloud-config-server.

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

En su clase Java de configuración utilice la anotación @Configuration y @EnabledConfigServer. 

```java
@Configuration
@SpringBootApplication
@EnableConfigServer
public class ConfiguracionServicioApplication {

	public static void main(String[] args) {
		SpringApplication.run(ConfiguracionServicioApplication.class, args);
	}
}
```

Configure su archivo yaml de la siguiente forma:

```yaml
server:
  port: 8080
spring:
  application:
    name: configuracion-service
  cloud:
    config:
      server:
        git:
          uri: https://github.com/your-domain-github/spring-api-microservices
          search-paths: configuracion-datos
        default-label: main
```

## Configuración Cliente

Si esta utilizando Java 8 asegúrese se usar en su archivo POM.XML una versión compatible de spring-cloud con Java 8.
Nota: para Java 17 utilice versiones de spring cloud superiores a 2022.0.x.

```xml
<properties>
    <java.version>1.8</java.version>
    <spring-cloud.version>2020.0.3</spring-cloud.version>
</properties>
```

Adicione en su archivo POM.XML las dependencias maven: spring-cloud-starter-config y spring-cloud-starter-bootstrap.

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bootstrap</artifactId>
</dependency>
```

Configure su archivo de propiedades bootstrap.yaml de la siguiente forma:

```yaml
spring:
  application:
    name: your-name-service   
  cloud:
    config:
      enabled: true
      uri: http://localhost:8080
```

Cuando se realice el proceso de despliegue del proyecto cliente sucederá lo siguiente: 
- El proyecto cliente se conectará automáticamente al servidor de configuraciones a través de la URI establecida en el archivo bootstrap.yaml.
- El servidor de configuraciones descargará y actualizará la configuración desde GIT en el proyecto cliente.
- El proyecto cliente se desplegará con la configuración suministrada por el servidor.

## Archivo GIT de Configuración

Utilice su cuenta GIT para crear un repositorio donde gestione sus archivos de configuración.

Ejemplo de un archivo de configuración:

your-name-service.yaml:

```yaml
server:
  port: ${server_port:1010}
```

## Recomendaciones

Algunas recomendaciones para esta arquitectura de despliegue:
- Si requiere alguna configuracion específica puede re-escribir el archivo de propiedades de su proyecto cliente.
- Asegúrese de no establecer configuraciones ambiguas con el servidor de configuraciones o causará conflictos en el despliegue. 
- Si está utilizando un administrador de despliegues (como Kubernetes) es recomendable que realice toda la configuración a través del archivo del despliegue del contenedor para cada proyecto independiente.
- Puede validar si su administrador de despliegues puede conectar y centralizar estas configuraciones usando un repositorio GIT.

```javascript
Estas notas son gracias a:
- Cursos y Tutoriales de Spring Boot realizados en Internet y basados en https://spring.io
- Experiencias propias de Laboratorio

Puede consultar Github -> https://github.com/fhantoing/computerx-blogposts
Palabras clave: Microservicios, Maven, Java, Spring Boot, Spring Cloud.
```
