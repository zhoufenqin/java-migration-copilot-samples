# Dependency Map

The Asset Manager project is a two-module Maven application (web + worker) with 10 unique declared external dependencies managed via the Spring Boot 2.7.18 parent BOM.

## Dependencies

```mermaid
flowchart LR
    Parent["Spring Boot Parent BOM v2.7.18"]
    WebApp["assets-manager-web"]
    WorkerApp["assets-manager-worker"]

    subgraph Web["Web Frameworks"]
        SpringWeb["Spring Boot Web (MVC)"]
        Thymeleaf["Thymeleaf 3.0.x"]
        SpringCore["Spring Boot Core"]
    end
    subgraph DB["Database / ORM"]
        SpringJPA["Spring Data JPA (Hibernate)"]
        PgDriver["PostgreSQL Driver 42.x"]
    end
    subgraph Messaging["Messaging"]
        SpringAMQP["Spring Boot AMQP (RabbitMQ)"]
    end
    subgraph Cloud["Cloud Storage"]
        AWSS3["AWS SDK S3 v2.25.13"]
    end
    subgraph Util["Utilities"]
        Lombok["Lombok 1.18.x"]
        Jackson["Jackson Databind 2.13.x"]
        DevTools["Spring Boot DevTools"]
    end

    Parent -.->|"manages versions"| SpringWeb
    Parent -.->|"manages versions"| Thymeleaf
    Parent -.->|"manages versions"| SpringCore
    Parent -.->|"manages versions"| SpringJPA
    Parent -.->|"manages versions"| PgDriver
    Parent -.->|"manages versions"| SpringAMQP
    Parent -.->|"manages versions"| Lombok
    Parent -.->|"manages versions"| Jackson

    WebApp -->|"web"| Web
    WebApp -->|"persistence"| DB
    WebApp -->|"messaging"| Messaging
    WebApp -->|"storage"| Cloud
    WebApp -->|"utilities"| Util

    WorkerApp -->|"core"| SpringCore
    WorkerApp -->|"persistence"| DB
    WorkerApp -->|"messaging"| Messaging
    WorkerApp -->|"storage"| Cloud
    WorkerApp -->|"utilities"| Util
```
