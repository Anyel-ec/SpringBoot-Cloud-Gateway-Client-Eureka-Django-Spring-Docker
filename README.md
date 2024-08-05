# Spring Cloud Gateway

This project sets up an API Gateway using Spring Cloud Gateway to route requests to two microservices: one built with Django and the other with Spring Boot. The Gateway also integrates with Eureka for service discovery.

## Project Structure

### Dependencies

The `pom.xml` file includes the following dependencies:

- Spring Cloud Gateway
- Spring Cloud Netflix Eureka Client
- Spring Boot Devtools (optional)
- Spring Boot Starter Web
- Spring Boot Starter Test
- Reactor Test

### Dockerfile

The `Dockerfile` is set up to build and run the Spring Cloud Gateway application.

### Gateway Configuration

The `application.properties` file includes routes for the Django and Spring Boot microservices and CORS configuration.

## Getting Started

### Prerequisites

- Java 17
- Maven
- Docker
- Docker Compose (optional)

### Building the Project

To build the project, run the following command:

```sh
mvn clean install
```

### Running the Project

You can run the project using Docker:

```sh
docker build -t gateway .
docker run -p 8080:8080 gateway
```

Alternatively, you can run the project locally using Maven:

```sh
mvn spring-boot:run
```

### Configuring Routes

The routes for the Django and Spring Boot microservices are configured in the `application.properties` file:

```properties
# Django service configuration
spring.cloud.gateway.routes[0].id=django-service
spring.cloud.gateway.routes[0].uri=lb://DJANGO-SERVICE
spring.cloud.gateway.routes[0].predicates[0]=Path=/django/**
spring.cloud.gateway.routes[0].filters[0]=RewritePath=/django/(?<segment>.*), /${segment}

# Spring Boot service configuration
spring.cloud.gateway.routes[1].id=my-microservice
spring.cloud.gateway.routes[1].uri=lb://SOCIALMEDIA-PROJECT
spring.cloud.gateway.routes[1].predicates[0]=Path=/springboot/**
spring.cloud.gateway.routes[1].filters[0]=RewritePath=/springboot/(?<segment>.*), /${segment}

# CORS configuration
spring.cloud.gateway.globalcors.cors-configurations.[/**].allowed-origins=http://localhost:4200
spring.cloud.gateway.globalcors.cors-configurations.[/**].allowed-methods=GET,POST,PUT,DELETE,OPTIONS
spring.cloud.gateway.globalcors.cors-configurations.[/**].allowed-headers=*
spring.cloud.gateway.globalcors.cors-configurations.[/**].allow-credentials=true
```

## Service Discovery with Eureka

The project uses Eureka for service discovery. Make sure you have a Eureka server running and configure the Eureka client properties in the `application.properties` file:

```properties
spring.application.name=gateway
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/
```

## Logging

To enable detailed logging for debugging purposes, uncomment the following lines in `application.properties`:

```properties
#logging.level.org.springframework.cloud.gateway=DEBUG
#logging.level.reactor.netty.http.server=DEBUG
```

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or bug fixes.

## License

This project is licensed under the MIT License.
