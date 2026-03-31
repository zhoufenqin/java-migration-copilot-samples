# Project Facts: mi-sql-public-demo (sqldbmi)

> Generated from 36 fact analysis results on 2026-03-31

## Summary

| Category | Finding |
|----------|---------|
| **Application Name** | sqldbmi (Maven: `com.example:demo:1.0-SNAPSHOT`) |
| **Application Type** | Standalone Java Console Application |
| **Architecture Pattern** | Simple Monolith |
| **Runtime** | Java 17 |
| **Version** | 1.0-SNAPSHOT |
| **License** | MIT License (Microsoft Corporation) |
| **External Services** | Azure SQL Database |
| **Container** | None (not containerized) |
| **Security** | Azure Managed Identity (MSI) + TLS |

---

## Application Identity

### Application Name
- **Status:** success | **Confidence:** medium
- **Finding:** Application name is `sqldbmi` (from README) with Maven artifactId `demo`
- **Evidence:**
  - README.md title: `# sqldbmi`
  - pom.xml `<artifactId>`: `demo`
  - pom.xml `<groupId>`: `com.example`
  - Main class: `com.example.MainSQL`
  - Directory name: `mi-sql-public-demo`
- **Values:** `sqldbmi`, `demo (Maven artifactId)`, `com.example:demo:1.0-SNAPSHOT`

### Application Type
- **Status:** success | **Confidence:** high
- **Finding:** Standalone Java Console Application — connects to Azure SQL Database using Managed Identity
- **Evidence:**
  - Single main class: `com.example.MainSQL` with `public static void main(String[] args)`
  - No web framework dependencies (no Spring Boot, JAX-RS, Servlet API)
  - No HTTP server or REST API endpoint definitions
  - Connects to Azure SQL Database via JDBC and terminates after connection test
  - Maven shade plugin configured to produce executable JAR with `MainSQL` as entry point
- **Values:** `Console Application`, `Command-line Tool`

### Version Information
- **Status:** success | **Confidence:** high
- **Finding:** Application version is `1.0-SNAPSHOT` as declared in pom.xml
- **Evidence:**
  - pom.xml `<version>`: `1.0-SNAPSHOT`
  - pom.xml `<groupId>`: `com.example`, `<artifactId>`: `demo`
  - No git tags found in repository
  - No version in application.properties
- **Values:** `Version: 1.0-SNAPSHOT`, `Version type: snapshot (development)`, `Full coordinates: com.example:demo:1.0-SNAPSHOT`

---

## Architecture & Design

### Architecture Pattern
- **Status:** success | **Confidence:** high
- **Finding:** Simple Monolith — single-class console application with no architectural layering
- **Evidence:**
  - Single deployable unit: one Maven module producing a single JAR
  - Single Java class: `com.example.MainSQL` with all logic in `main()`
  - No MVC structure: no controllers, services, repositories, or view templates
  - No microservices indicators: no service discovery, API gateway, or multiple services
  - No event-driven patterns: no message brokers, event handlers, or pub/sub
  - Entire application logic is contained in ~30 lines of code
- **Values:** `Simple Monolith`, `Console Application`

### Communication Protocols
- **Status:** success | **Confidence:** high
- **Finding:** JDBC over TCP (port 1433) is the only communication protocol — used to connect to Azure SQL Database
- **Evidence:**
  - No `@RestController`, `@RequestMapping`, `HttpClient`, or `RestTemplate` usage
  - No gRPC `.proto` files found
  - No message queue dependencies in pom.xml
  - `MainSQL.java` uses `SQLServerDataSource` with JDBC URL: `jdbc:sqlserver://*.database.windows.net:1433`
  - Connection uses `ActiveDirectoryMSI` authentication over encrypted TLS (`encrypt=true`)
- **Values:** `JDBC over TCP port 1433`, `Azure SQL Database`, `TLS encrypted / ActiveDirectoryMSI`, `Outbound only`

### Application Port
- **Status:** not_applicable | **Confidence:** high
- **Finding:** No application ports — CLI console application with no HTTP server or network listener

### Profile Settings
- **Status:** not_applicable | **Confidence:** high
- **Finding:** No environment profiles detected — single configuration model using one `application.properties` file

### XML Configs
- **Status:** not_applicable | **Confidence:** high
- **Finding:** No XML application configuration files detected — only `pom.xml` (Maven build descriptor) found; application uses `application.properties` for runtime configuration

---

## Runtime Environment

### Runtime Environment
- **Status:** success | **Confidence:** medium
- **Finding:** Java 17
- **Evidence:** Maven/Gradle build file found with `maven.compiler.source=17`
- **Values:** `Java`, `Version: 17`

### Operating System
- **Status:** success | **Confidence:** medium
- **Finding:** Cross-platform Java application — no OS-specific configuration; JVM targets Linux for cloud deployment on Azure
- **Evidence:**
  - No Dockerfile with OS-specific base image
  - pom.xml: Java 17 (platform-independent JVM bytecode)
  - Azure SQL Database connection indicates cloud/Linux deployment
- **Values:** `Target OS: Linux (typical Azure Java deployment)`, `JVM: Platform-independent (Java 17 bytecode)`, `Architecture: x64`

### Hardware Requirements
- **Status:** not_applicable | **Confidence:** high
- **Finding:** No hardware requirements found — no documentation, container resource limits, or Kubernetes resource requests

### Startup Instrumentation
- **Status:** not_applicable | **Confidence:** high
- **Finding:** No structured logging framework, telemetry, or AOP detected — application uses `System.out.println` for basic console output only
- **Evidence:**
  - No SLF4J, Logback, Log4j2, or other logging dependencies in pom.xml
  - No OpenTelemetry, Application Insights, New Relic, or Datadog dependencies
  - No `@Aspect`, `@Before`, `@After`, `@Around` AOP annotations
  - `MainSQL.java` uses `System.out.println()` and `e.printStackTrace()`

### Embedded Language Usage
- **Status:** not_applicable | **Confidence:** high
- **Finding:** No embedded language usage detected — pure Java implementation with standard JDBC database connectivity only

### Testing Framework
- **Status:** not_applicable | **Confidence:** high
- **Finding:** No testing frameworks detected — no test files or test dependencies found

---

## Dependencies & Packaging

### Language Dependencies
- **Status:** success | **Confidence:** high
- **Finding:** 1 Java runtime dependency declared in `pom.xml`: Microsoft SQL Server JDBC driver. No container packaging exists.
- **Evidence:**
  - `com.microsoft.sqlserver:mssql-jdbc:10.2.0.jre11` (compile scope)
  - Build plugin: `maven-shade-plugin:3.2.4` produces uber-JAR
- **Values:**
  - `Dependency file: pom.xml (Maven)`
  - `Runtime dependencies: 1`
  - `Key dependency: com.microsoft.sqlserver:mssql-jdbc:10.2.0.jre11`
  - `Packaging: Uber-JAR via maven-shade-plugin`

### Servlet Container
- **Status:** not_applicable | **Confidence:** high
- **Finding:** Non-web application — no servlet container required

---

## External Services & Dependencies

### External Services
- **Status:** success | **Confidence:** high
- **Finding:** Azure SQL Database (Microsoft SQL Server) is the sole external service dependency
- **Evidence:**
  - `application.properties`: `AZURE_SQLDB_CONNECTIONSTRING=jdbc:sqlserver://${AZ_DATABASE_SERVER_NAME}.database.windows.net:1433;database=demo`
  - `pom.xml`: `com.microsoft.sqlserver:mssql-jdbc:10.2.0.jre11`
  - `MainSQL.java`: `SQLServerDataSource` with `ActiveDirectoryMSI` authentication
- **Values:** `Azure SQL Database`, `JDBC over TCP port 1433 with TLS`, `Managed Identity (ActiveDirectoryMSI)`, `Database name: demo`

### External Dependencies
- **Status:** success | **Confidence:** high
- **Finding:** One external system dependency: Azure SQL Database (Microsoft SQL Server hosted on Azure)
- **Values:**
  - `Azure SQL Database (Microsoft SQL Server on Azure) — database`
  - `Azure Managed Identity service (via AZURE_CLIENT_ID) — authentication`
  - `Count: 1 primary external system + Azure Identity platform`

### Environment Variables
- **Status:** success | **Confidence:** high
- **Finding:** 2 configuration properties used, sourced from `application.properties`
- **Values:**
  - `AZURE_SQLDB_CONNECTIONSTRING` — Azure SQL Database JDBC connection string
  - `AZURE_CLIENT_ID` — Azure Managed Identity client ID
  - `Categories: database, authentication`

---

## Security & Compliance

### Security Implementation
- **Status:** success | **Confidence:** high
- **Finding:** Security is implemented via Azure Managed Identity (MSI) for passwordless authentication and TLS encryption for database connections
- **Evidence:**
  - `authentication=ActiveDirectoryMSI` — passwordless identity-based access
  - `encrypt=true` — TLS encryption enforced
  - `trustServerCertificate=false` — server certificate validation enabled
  - `hostNameInCertificate=*.database.windows.net` — hostname verification enabled
  - No hardcoded passwords or credentials in source code
- **Values:**
  - `Authentication: Azure Managed Identity (ActiveDirectoryMSI) — passwordless`
  - `Transport: TLS encrypted connection to Azure SQL Database (port 1433)`
  - `Certificate validation: enabled`
  - `Credential management: no secrets in code, identity-based access`

### Licensing Information
- **Status:** success | **Confidence:** high
- **Finding:** MIT License — Copyright Microsoft Corporation, applied at the repository level
- **Values:**
  - `Primary license: MIT License`
  - `Copyright holder: Microsoft Corporation`
  - `Dependency license: mssql-jdbc is MIT (Microsoft)`
  - `License compatibility: MIT — permissive, compatible with all common open-source licenses`

### Compliance Requirements
- **Status:** not_applicable | **Confidence:** medium
- **Finding:** No compliance requirements detected — no GDPR, HIPAA, PCI-DSS, or SOX indicators found

### Data Classification
- **Status:** not_applicable | **Confidence:** medium
- **Finding:** No data classification markers detected — application only tests database connectivity, handles no PII, PHI, or payment data

### Health Checks
- **Status:** not_applicable | **Confidence:** high
- **Finding:** No health checks configured — application is a short-lived console process with no persistent runtime monitoring

---

## Container & Infrastructure

> All container-related facts are **not applicable** — the application is not containerized and has no orchestration configuration.

| Fact | Status | Finding |
|------|--------|---------|
| Container Engine | not_applicable | No container engine found |
| Base Image | not_applicable | No Dockerfile or Containerfile present |
| Image Size | not_applicable | No container image to analyze |
| Image Layers | not_applicable | No Dockerfile instructions to count |
| Multi-stage Build | not_applicable | No Dockerfile found |
| Container Version | not_applicable | No container runtime version to identify |
| Volume Mounts | not_applicable | No volume configurations found |
| Orchestration Tool | not_applicable | No orchestration tool configured |
| Service Definition | not_applicable | No Docker Compose, Kubernetes, or Helm configs |
| Resource Limits | not_applicable | No container resource constraints |
| Network Settings | not_applicable | No container network configuration |
| System Packages | not_applicable | No Dockerfile package installations |

---

## Key Observations for Modernization

1. **No containerization** — The application needs a Dockerfile and container build pipeline before it can be deployed to Azure Container Apps, AKS, or App Service containers.
2. **Single external dependency** — Only Azure SQL Database via JDBC; this simplifies migration.
3. **Strong security posture** — Already uses Azure Managed Identity and TLS, aligning with cloud-native best practices.
4. **No tests** — No test suite exists; tests should be added before migration to validate behavior.
5. **No logging framework** — `System.out.println` should be replaced with a structured logging framework (e.g., SLF4J + Logback) for cloud observability.
6. **Java 17** — A supported LTS version, compatible with all current Azure compute targets.
7. **Minimal codebase** — ~30 lines of Java make this a strong candidate for quick containerization and cloud migration.
