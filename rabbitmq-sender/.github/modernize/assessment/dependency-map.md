# Dependency Map

The `messaging-rabbitmq` Spring Boot 3.3.0 project declares 4 runtime/provided dependencies (test-scoped excluded), governed by the Spring Boot parent BOM and the Azure Spring Cloud BOM.

## Dependencies

```mermaid
flowchart LR
    App["messaging-rabbitmq\nv0.0.1-SNAPSHOT"]

    subgraph BOM["BOM / Parent"]
        SBParent["spring-boot-starter-parent\nv3.3.0"]
        AzureBOM["spring-cloud-azure-dependencies\nv5.14.0"]
    end
    subgraph Messaging["Messaging"]
        ServiceBus["spring-messaging-azure-servicebus\nv5.14.0 (BOM-managed)"]
    end
    subgraph Cloud["Azure Cloud"]
        AzureStarter["spring-cloud-azure-starter\nv5.14.0 (BOM-managed)"]
    end
    subgraph Util["Utilities"]
        Lombok["Lombok v1.18.24\n(provided)"]
        Jackson["jackson-databind\n(BOM-managed)"]
    end

    App -->|"messaging"| Messaging
    App -->|"cloud integration"| Cloud
    App -->|"utilities"| Util
    SBParent -.->|"manages"| Jackson
    AzureBOM -.->|"manages"| ServiceBus
    AzureBOM -.->|"manages"| AzureStarter
```
