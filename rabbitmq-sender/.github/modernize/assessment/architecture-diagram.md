# Architecture Diagram

A Spring Boot 3.3.0 Java 17 application that sends messages to Azure Service Bus queues using Azure Spring Cloud SDK with Managed Identity authentication.

## Application Architecture

```mermaid
flowchart TD
    subgraph Startup["Startup Layer"]
        Main["MessagingRabbitmqApplication\nSpring Boot 3.3.0"]
    end
    subgraph App["Application Layer - Java 17"]
        Producer["Producer\nMessage Sender"]
    end
    subgraph Azure["Azure SDK Layer"]
        SBT["ServiceBusTemplate\nspring-messaging-azure-servicebus"]
        SBAClient["ServiceBusAdministrationClient\nQueue Management"]
    end
    subgraph External["External Services"]
        ASB[("Azure Service Bus\nqueue1 / queue2")]
        MI["Azure Managed Identity\nAuthentication"]
    end

    Main -->|"runs"| Producer
    Main -->|"creates beans"| SBAClient
    Main -->|"auto-configures"| SBT
    Producer -->|"send messages"| SBT
    SBT -->|"publish to queues"| ASB
    SBAClient -->|"create/get queues"| ASB
    MI -->|"authenticates"| SBAClient
    MI -->|"authenticates"| SBT
```

## Component Relationships

```mermaid
flowchart LR
    subgraph Startup["Startup"]
        AppMain["MessagingRabbitmqApplication"]
    end
    subgraph Business["Business Logic"]
        Prod["Producer"]
    end
    subgraph Infra["Infrastructure"]
        SBTemplate["ServiceBusTemplate"]
        AdminClient["ServiceBusAdministrationClient"]
    end
    subgraph Config["Configuration"]
        AppProps["application.properties\nManaged Identity + Namespace"]
    end

    AppMain -->|"invokes run"| Prod
    AppMain -->|"bean: adminClient"| AdminClient
    AppMain -->|"bean: queue1/queue2"| AdminClient
    Prod -->|"autowired"| SBTemplate
    SBTemplate -->|"sends to queue1/queue2"| AdminClient
    AppProps -.->|"configures"| SBTemplate
    AppProps -.->|"configures"| AdminClient
```
