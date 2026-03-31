# Dependency Map

The `todo-web-api-use-oracle-db` project declares 6 runtime/compile dependencies (excluding test scope), with versions managed by the Spring Boot 3.2.4 parent BOM.

## Dependencies

```mermaid
flowchart LR
    App["todo-web-api-use-oracle-db\nv0.0.1-SNAPSHOT"]

    subgraph BOM["Parent BOM"]
        ParentBOM["spring-boot-starter-parent\nv3.2.4"]
    end

    subgraph Web["Web Frameworks"]
        SpringWeb["Spring Boot Starter Web\n(managed by BOM)"]
        SpringVal["Spring Boot Starter Validation\n(managed by BOM)"]
    end

    subgraph DB["Database / ORM"]
        SpringJPA["Spring Boot Starter Data JPA\n(managed by BOM)"]
        OracleDriver["Oracle JDBC Driver ojdbc11\n(managed by BOM)"]
    end

    subgraph Util["Utilities"]
        Lombok["Lombok\n(managed by BOM)"]
        DevTools["Spring Boot DevTools\n(managed by BOM)"]
    end

    App -.->|"inherits"| ParentBOM
    ParentBOM -.->|"manages versions"| SpringWeb
    ParentBOM -.->|"manages versions"| SpringVal
    ParentBOM -.->|"manages versions"| SpringJPA
    ParentBOM -.->|"manages versions"| OracleDriver
    ParentBOM -.->|"manages versions"| Lombok
    ParentBOM -.->|"manages versions"| DevTools
    App -->|"web"| Web
    App -->|"persistence"| DB
    App -->|"utilities"| Util
```
