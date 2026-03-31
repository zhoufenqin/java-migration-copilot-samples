# Architecture Diagram

This Spring Boot 3.2 REST API application manages Todo items using an Oracle Database, exposing CRUD and search endpoints via HTTP on port 8080.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        HTTP["HTTP Client"]
    end
    subgraph App["Application Layer - Spring Boot 3.2 / Java 17"]
        Controller["TodoController\n(REST API - /api/todos)"]
        Service["TodoService\n(Business Logic)"]
        Util["OracleSqlDemonstrator\n(Raw SQL Utility)"]
    end
    subgraph Data["Data Layer"]
        Repo["TodoRepository\n(Spring Data JPA)"]
        JPA["EntityManager\n(Native Queries)"]
        JDBC["JdbcTemplate\n(Raw JDBC)"]
        DB[("Oracle Database\nXEPDB1 @ port 1521")]
    end

    HTTP -->|"HTTP REST requests\nport 8080"| Controller
    Controller -->|"delegates"| Service
    Controller -->|"delegates"| Util
    Service -->|"CRUD operations"| Repo
    Service -->|"native Oracle SQL"| JPA
    Util -->|"raw Oracle SQL / PL-SQL"| JDBC
    Repo -->|"JPA queries"| DB
    JPA -->|"native SQL"| DB
    JDBC -->|"JDBC"| DB
```

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation Layer"]
        TC["TodoController"]
    end
    subgraph Business["Business Logic Layer"]
        TS["TodoService"]
        OSD["OracleSqlDemonstrator"]
    end
    subgraph DataAccess["Data Access Layer"]
        TR["TodoRepository"]
        EM["EntityManager"]
        JT["JdbcTemplate"]
    end
    subgraph Domain["Domain Model"]
        TI["TodoItem"]
    end

    TC -->|"delegates CRUD"| TS
    TC -->|"delegates raw queries"| OSD
    TS -->|"repository calls"| TR
    TS -->|"native queries"| EM
    OSD -->|"raw JDBC"| JT
    TR -->|"manages"| TI
    EM -->|"maps results"| TI
```
