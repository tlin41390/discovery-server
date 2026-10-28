# Discovery Server

A standalone [Netflix Eureka](https://github.com/Netflix/eureka) service registry built with Spring Boot and Spring Cloud. Microservices register themselves here and look each other up by name instead of by hard-coded host and port.

## Tech stack

- Java 25
- Spring Boot 4.1.2
- Spring Cloud 2025.1.4 (`spring-cloud-starter-netflix-eureka-server`)
- Maven (wrapper included)

## Running locally

```bash
./mvnw spring-boot:run
```

On Windows:

```powershell
.\mvnw.cmd spring-boot:run
```

When the server is up, open the Eureka dashboard at <http://localhost:8761>.

## Configuration

`src/main/resources/application.properties`:

| Property | Value | Purpose |
| --- | --- | --- |
| `spring.application.name` | `discovery-server` | Service name |
| `server.port` | `8761` | Eureka's conventional port |
| `eureka.client.register-with-eureka` | `false` | The server does not register itself as a client |
| `eureka.client.fetch-registry` | `false` | The server does not pull a registry from another server |

## Registering a client service

In the client service, add the Eureka client dependency:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

Then point it at this server:

```properties
spring.application.name=my-service
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/
```

After the client starts, it appears on the dashboard under its application name.

## Building and testing

```bash
./mvnw clean package   # builds target/discovery-server-0.0.1-SNAPSHOT.jar
./mvnw test
```
