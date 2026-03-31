# Asset Manager — Project Facts

Consolidated assessment report generated from 36 fact analyses of the `asset-manager` Java application.

---

## Application Identity

| Fact | Finding | Confidence |
|------|---------|------------|
| **Application Name** | Asset Manager (`assets-manager-web` + `assets-manager-worker`) | High |
| **Application Type** | Web Application (Spring MVC + Thymeleaf) + Background Worker (AMQP consumer) | High |
| **Application Port** | Port 8080 (HTTP, web module only); worker has no port | High |
| **Version** | 0.0.1-SNAPSHOT (Maven, pre-release) | High |

---

## Runtime & Technology Stack

| Fact | Finding | Confidence |
|------|---------|------------|
| **Runtime Environment** | Java 8 (source), Java 21 JRE target (Eclipse Temurin alpine) | High |
| **Architecture Pattern** | Multi-module Layered MVC + Event-Driven (web + worker) | High |
| **Servlet Container** | Embedded Apache Tomcat 9.0.x (Servlet 4.0 / Java EE 8, `javax.servlet`) | High |
| **XML Configs** | None — pure Spring Boot annotation-based configuration | High |
| **Profile Settings** | 3 Spring profiles: `dev` (local filesystem), `!dev` (production/AWS S3), `backup` (optional) | High |
| **Embedded Language Usage** | None — pure Java implementation | High |

**Framework Details:**
- Spring Boot 2.7.18 (parent BOM)
- Spring MVC + Thymeleaf (web module)
- Spring Data JPA + Hibernate ORM
- Spring AMQP (RabbitMQ integration)
- AWS SDK for Java v2 (v2.25.13)
- Java source: Java 8 (with `javax.servlet`, `javax.annotation`)

---

## Container & Deployment

| Fact | Finding | Confidence |
|------|---------|------------|
| **Container Engine** | Docker (local dev via `docker run`; production via Azure Container Registry) | High |
| **Base Image** | `eclipse-temurin:21-jre-alpine` (generated at deploy time) | Medium |
| **Image Size** | Web: ~290–310 MB; Worker: ~275–295 MB (estimated) | Medium |
| **Image Layers** | ~7–9 layers per image (base + WORKDIR + COPY) | Medium |
| **Multi-stage Build** | No — single-stage Dockerfiles | High |
| **Container Version** | Not explicitly pinned; Docker Desktop (any modern version) | Low |
| **System Packages** | None installed — minimal JRE base image only | High |
| **Volume Mounts** | None — production uses AWS S3; dev uses local filesystem (no container volumes) | High |

**Deployment:**
- Production: Azure Container Apps (`az containerapp create`)
- Local dev: Spring Boot processes via Maven (`mvnw spring-boot:run -Pdev`)
- Infrastructure (local): PostgreSQL and RabbitMQ via `docker run`

---

## Infrastructure & Orchestration

| Fact | Finding | Confidence |
|------|---------|------------|
| **Orchestration Tool** | Azure Container Apps (production); no Compose or Kubernetes | High |
| **Service Definition** | Imperative Azure CLI scripting (`deploy-to-azure.sh`); no declarative manifests | High |
| **Network Settings** | Default Docker bridge (local); Azure Container Apps external ingress on port 8080 (web) | High |
| **Resource Limits** | Not explicitly set; ACA defaults (0.25 vCPU / 0.5 Gi per replica); 1–3 replicas | Medium |
| **Operating System** | Alpine Linux (container); cross-platform (local: Linux/macOS .sh + Windows .cmd) | High |
| **Hardware Requirements** | Not documented; inferred: ACA defaults, ~4 GB RAM for local dev | High |

---

## External Services & Dependencies

| Fact | Finding | Confidence |
|------|---------|------------|
| **External Services** | PostgreSQL, RabbitMQ, AWS S3 | High |
| **External Dependencies** | PostgreSQL (JDBC/5432), RabbitMQ (AMQP/5672), AWS S3 (SDK v2), JDK 8, Maven 3.6+ | High |
| **Communication Protocols** | HTTP/REST (browser ↔ web), AMQP/RabbitMQ (web ↔ worker), HTTPS (AWS S3 SDK), JDBC/TCP (PostgreSQL) | High |

---

## Configuration & Environment

| Fact | Finding | Confidence |
|------|---------|------------|
| **Environment Variables** | AWS credentials, RabbitMQ, DB connection in `application.properties`; Azure env vars at deploy time | Medium |
| **Health Checks** | None — no Spring Actuator, no HEALTHCHECK instruction, no K8s probes | High |

---

## Security

| Fact | Finding | Confidence |
|------|---------|------------|
| **Security Implementation** | Minimal — no Spring Security, no authentication/authorization, no HTTPS config, no CSRF; AWS credentials stored as plaintext | High |
| **Compliance Requirements** | None — sample/workshop app handling image files only (no PII/PHI/PCI) | Medium |
| **Data Classification** | Internal — image files and metadata only; no sensitive personal data | High |
| **Licensing Information** | MIT License (Copyright © 2025 Menghua Xiao) | High |

---

## Testing

| Fact | Finding | Confidence |
|------|---------|------------|
| **Testing Framework** | JUnit 5 (Jupiter) + Mockito via `spring-boot-starter-test`; 1 test class (worker module only) | High |
| **Startup Instrumentation** | SLF4J + Logback (default); custom `FileOperationLoggingInterceptor`; no APM/telemetry | Medium |

---

## Key Migration Findings

Based on the AppCAT assessment and fact analysis, the following are the primary migration concerns:

1. **Java 8 → Java 21 upgrade required** — current source uses `java.version=8` and old APIs (`javax.*` namespace)
2. **Spring Boot 2.7 → 3.x upgrade required** — needed for Azure Container Apps best practices; requires `javax.servlet` → `jakarta.servlet` migration
3. **AWS S3 → Azure Blob Storage migration** — `AwsS3Service` in web and `S3FileProcessingService` in worker need replacement
4. **RabbitMQ → Azure Service Bus migration** — AMQP configuration and `RabbitTemplate`/`@RabbitListener` need migration
5. **PostgreSQL → Azure Database for PostgreSQL** — connection strings and auth need updating (managed identity)
6. **No health checks** — Spring Boot Actuator required for Azure Container Apps readiness/liveness probes
7. **No security** — authentication, secrets management (no plaintext credentials), and HTTPS termination need attention
8. **Plaintext credentials in application.properties** — must migrate to Azure Managed Identity / Key Vault

---

*Generated: 2026-03-31 | Facts directory: `asset-manager/.github/modernize/assessment/facts/` | Total facts collected: 36*
