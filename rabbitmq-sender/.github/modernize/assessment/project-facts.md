# Project Facts: messaging-rabbitmq (RabbitMQ Sender)

Generated from assessment of `rabbitmq-sender` sub-directory.

---

## Application Identity

| Fact | Value | Confidence |
|------|-------|------------|
| **Application Name** | `messaging-rabbitmq` (display: "RabbitMQ Sender") | High |
| **Application Type** | Background Service / Message Producer (standalone Spring Boot) | High |
| **Version** | `0.0.1-SNAPSHOT` (Maven development snapshot) | High |
| **Architecture Pattern** | Event-Driven Single-Service (outbound message producer) | High |

---

## Runtime & Language

| Fact | Value | Confidence |
|------|-------|------------|
| **Runtime Environment** | Java 17 (LTS) with Spring Boot 3.3.0 | High |
| **Build Tool** | Maven (spring-boot-maven-plugin, fat JAR packaging) | High |
| **Servlet Container** | Not applicable — no web components, standalone JAR | High |
| **XML Configuration** | Not applicable — annotation-based config only (`@SpringBootApplication`, `@Bean`) | High |
| **Embedded Language Usage** | None — pure Java, no scripting engines or native libraries | High |

---

## Dependencies

| Fact | Value | Confidence |
|------|-------|------------|
| **Language Dependencies** | 4 runtime/provided: `spring-cloud-azure-starter`, `spring-messaging-azure-servicebus`, `lombok:1.18.24`, `jackson-databind` | High |
| **BOM Management** | `spring-boot-starter-parent:3.3.0` + `spring-cloud-azure-dependencies:5.14.0` | High |
| **Testing Framework** | No tests implemented; `spring-boot-starter-test` (JUnit 5 + Mockito) available but unused | High |

---

## Communication & External Services

| Fact | Value | Confidence |
|------|-------|------------|
| **Communication Protocols** | AMQP 1.0 (outbound only via Azure Service Bus) | High |
| **External Services** | Azure Service Bus (queues: `queue1`, `queue2`) | High |
| **External Dependencies** | Azure Service Bus + Azure Managed Identity | High |
| **Application Port** | Not applicable — no inbound network ports | High |

---

## Configuration

| Fact | Value | Confidence |
|------|-------|------------|
| **Environment Variables** | `SPRING_CLOUD_AZURE_SERVICEBUS_NAMESPACE` (required), `SPRING_CLOUD_AZURE_CREDENTIAL_CLIENT_ID` (optional) | Medium |
| **Profile Settings** | No environment profiles — single `application.properties` configuration | High |

---

## Security

| Fact | Value | Confidence |
|------|-------|------------|
| **Security Implementation** | Azure Managed Identity (passwordless auth) + TLS 1.2+ in transit (enforced by Azure SDK) | High |
| **Licensing** | MIT License (Microsoft Corporation, repository root) | High |
| **Compliance Requirements** | No explicit compliance requirements detected | Medium |
| **Data Classification** | No sensitive data markers (PII/PHI/PCI) detected — demo application sends test strings | Medium |

---

## Containerization & Deployment

| Fact | Value | Confidence |
|------|-------|------------|
| **Container Engine** | Not applicable — no Dockerfile or Containerfile | High |
| **Base Image** | Not applicable — no container image defined | High |
| **Image Size** | Not applicable | High |
| **Image Layers** | Not applicable | High |
| **Multi-stage Build** | Not applicable | High |
| **Container Version** | Not applicable | High |
| **Volume Mounts** | Not applicable — stateless application | High |
| **System Packages** | Not applicable — no container | High |
| **Orchestration Tool** | Not applicable — no Kubernetes, Docker Compose, or Swarm | High |
| **Service Definition** | Not applicable — no orchestration files | High |
| **Resource Limits** | Not applicable — no container constraints configured | High |
| **Network Settings** | Not applicable — outbound-only via Azure SDK | High |

---

## Infrastructure

| Fact | Value | Confidence |
|------|-------|------------|
| **Operating System** | Cross-platform JVM; Linux recommended for Azure cloud deployment | Medium |
| **Hardware Requirements** | Not documented; typical Spring Boot: ≥512MB RAM, ≥0.5 CPU | High |
| **Health Checks** | None configured | High |

---

## Observability & Development

| Fact | Value | Confidence |
|------|-------|------------|
| **Startup Instrumentation** | Minimal — `System.out.println()` only; no structured logging, APM, or AOP | High |
| **Testing Framework** | No tests implemented (JUnit 5 dependency available but unused) | High |
| **Embedded Language Usage** | None | High |

---

## AppCAT Assessment Summary

AppCAT analysis detected **3 issues** across **6 incidents** with a total effort of **43 story points**, all categorized as **mandatory** severity:

| Category | Issues | Incidents |
|----------|--------|-----------|
| Framework Upgrade | 2 | 5 |
| Containerization | 1 | 1 |

**Targets assessed:** Azure Kubernetes Service, Azure App Service, Azure Container Apps

---

## Key Observations

1. **Already migrated to Azure Service Bus** — The application name says "rabbitmq" but the code has been migrated from RabbitMQ (AMQP 0-9-1) to Azure Service Bus (AMQP 1.0) using the Azure Spring Cloud SDK.
2. **Passwordless authentication** — Uses Azure Managed Identity, which is a security best practice for Azure-hosted workloads.
3. **No containerization** — A Dockerfile and container image should be created before deploying to AKS or Azure Container Apps.
4. **No tests** — Zero test coverage; test infrastructure (JUnit 5 + Mockito) is available via the existing dependency.
5. **Minimal logging** — Uses `System.out.println()` instead of SLF4J/Logback; structured logging should be added.
6. **Framework upgrade recommended** — AppCAT detected Spring framework version issues that need addressing for cloud deployment.
