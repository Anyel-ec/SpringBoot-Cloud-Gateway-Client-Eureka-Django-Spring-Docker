# Spring Cloud Gateway

Este proyecto configura un API Gateway utilizando Spring Cloud Gateway para enrutar solicitudes a dos microservicios: uno construido con Django y otro con Spring Boot. El Gateway también se integra con Eureka para el descubrimiento de servicios.

## Estructura del Proyecto

### Dependencias

El archivo `pom.xml` incluye las siguientes dependencias:

- Spring Cloud Gateway
- Spring Cloud Netflix Eureka Client
- Spring Boot Devtools (opcional)
- Spring Boot Starter Web
- Spring Boot Starter Test
- Reactor Test

### Dockerfile

El `Dockerfile` está configurado para construir y ejecutar la aplicación Spring Cloud Gateway.

### Configuración del Gateway

El archivo `application.properties` incluye rutas para los microservicios de Django y Spring Boot, así como la configuración de CORS.

## Primeros Pasos

### Prerrequisitos

- Java 17
- Maven
- Docker
- Docker Compose (opcional)

### Construir el Proyecto

Para construir el proyecto, ejecute el siguiente comando:

```sh
mvn clean install
```

### Ejecutar el Proyecto

Puede ejecutar el proyecto utilizando Docker:

```sh
docker build -t gateway .
docker run -p 8080:8080 gateway
```

Alternativamente, puede ejecutar el proyecto localmente utilizando Maven:

```sh
mvn spring-boot:run
```

### Configuración de Rutas

Las rutas para los microservicios de Django y Spring Boot están configuradas en el archivo `application.properties`:

```properties
# Configuración del servicio Django
spring.cloud.gateway.routes[0].id=django-service
spring.cloud.gateway.routes[0].uri=lb://DJANGO-SERVICE
spring.cloud.gateway.routes[0].predicates[0]=Path=/django/**
spring.cloud.gateway.routes[0].filters[0]=RewritePath=/django/(?<segment>.*), /${segment}

# Configuración del servicio Spring Boot
spring.cloud.gateway.routes[1].id=my-microservice
spring.cloud.gateway.routes[1].uri=lb://SOCIALMEDIA-PROJECT
spring.cloud.gateway.routes[1].predicates[0]=Path=/springboot/**
spring.cloud.gateway.routes[1].filters[0]=RewritePath=/springboot/(?<segment>.*), /${segment}

# Configuración de CORS
spring.cloud.gateway.globalcors.cors-configurations.[/**].allowed-origins=http://localhost:4200
spring.cloud.gateway.globalcors.cors-configurations.[/**].allowed-methods=GET,POST,PUT,DELETE,OPTIONS
spring.cloud.gateway.globalcors.cors-configurations.[/**].allowed-headers=*
spring.cloud.gateway.globalcors.cors-configurations.[/**].allow-credentials=true
```

## Descubrimiento de Servicios con Eureka

El proyecto utiliza Eureka para el descubrimiento de servicios. Asegúrese de tener un servidor Eureka en funcionamiento y configure las propiedades del cliente Eureka en el archivo `application.properties`:

```properties
spring.application.name=gateway
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/
```

## Logging

Para habilitar el registro detallado con fines de depuración, descomente las siguientes líneas en `application.properties`:

```properties
#logging.level.org.springframework.cloud.gateway=DEBUG
#logging.level.reactor.netty.http.server=DEBUG
```

## Contribuciones

¡Las contribuciones son bienvenidas! Por favor, abra un issue o envíe un pull request para cualquier mejora o corrección de errores.

## Licencia

Este proyecto está licenciado bajo la Licencia MIT.
