# Project Facts Report

**Project**: todo-web-api-use-oracle-db  
**Generated**: 2026-03-31  
**Assessment Source**: 36 fact skill results from `.github/modernize/assessment/facts/`

---

## Summary

| Category | Finding | Confidence |
|----------|---------|------------|
| Application Name | todo-web-api-use-oracle-db | High |
| Application Type | REST API | High |
| Runtime Environment | Java 17 + Spring Boot 3.2.4 | High |
| Architecture Pattern | Layered Monolith | High |
| External Services | Oracle Database 21c XE | High |
| Container Engine | Docker (for Oracle DB only) | Medium |
| Containerized | No | High |
| Orchestration | None | High |
| Security | None (HTTP only, hardcoded credentials) | High |
| Testing | No test files (JUnit 5 declared) | High |

---

## 1. Application Identity

### Application Name
- **Name**: `todo-web-api-use-oracle-db`
- **Display Name**: Todo Web API with Oracle Database
- **Group ID**: `com.microsoft.migration`
- **Version**: `0.0.1-SNAPSHOT`
- **Description**: Todo Web API using Oracle Database

### Application Type
- **Type**: REST API
- **Framework**: Spring Boot 3.2.4 with Spring MVC
- **Entry Point**: `TodoApplication.java` (`@SpringBootApplication`)

### Version Information
- **Application Version**: 0.0.1-SNAPSHOT (development pre-release)
- **Spring Boot**: 3.2.4
- **Java**: 17 (LTS)
- **Maven**: 3.8+ (per prerequisites)

---

## 2. Runtime & Technology Stack

### Runtime Environment
- **Language**: Java 17 (LTS)
- **Framework**: Spring Boot 3.2.4
- **Deployment**: Spring Boot executable fat JAR or Maven plugin
- **Build Tool**: Maven

### Servlet Container
- **Container**: Apache Tomcat 10.1.x (embedded)
- **Servlet API**: Jakarta Servlet 6.0 (Jakarta EE 10)
- **Model**: Self-contained fat JAR — no external container needed

### Language Dependencies (pom.xml)
| Dependency | Scope | Purpose |
|-----------|-------|---------|
| spring-boot-starter-web | compile | HTTP REST API, embedded Tomcat |
| spring-boot-starter-data-jpa | compile | ORM / data access |
| com.oracle.database.jdbc:ojdbc11 | runtime | Oracle JDBC driver |
| spring-boot-starter-validation | compile | Bean validation |
| org.projectlombok:lombok | optional | Code generation |
| spring-boot-devtools | runtime/optional | Hot reload |
| spring-boot-starter-test | test | JUnit 5, Mockito, AssertJ |

---

## 3. Architecture

### Architecture Pattern
- **Pattern**: Layered Monolith (REST API)
- **Layers**:
  - Presentation: `TodoController` (`@RestController`, `/api/todos`)
  - Business: `TodoService`, `OracleSqlDemonstrator`
  - Data Access: `TodoRepository` (Spring Data JPA), `EntityManager` (native queries), `JdbcTemplate` (raw JDBC)
  - Domain: `TodoItem` (JPA entity)

### Communication Protocols
- **Inbound**: HTTP/REST on port 8080 (13 endpoints: GET, POST, PUT, DELETE)
- **Outbound**: JDBC/TCP to Oracle Database (localhost:1521)

### External Services
- **Oracle Database 21c XE** — sole external dependency
  - Host: `localhost`, Port: `1521`, Service: `XEPDB1`
  - Driver: ojdbc11 (JDBC Thin)
  - Authentication: hardcoded username/password (security concern)

---

## 4. Configuration

### Application Port
- **HTTP Port**: 8080 (configured in `application.yaml`)
- **No HTTPS** configured

### Environment Variables
- No environment variables configured
- All configuration hardcoded in `application.yaml`
- **Risk**: Database credentials stored in plain text

### Profile Settings
- **No Spring profiles defined** — single configuration model
- No `application-dev.yml`, `application-prod.yml` etc.

### XML Configuration
- No application XML configs — uses annotation-based configuration exclusively
- Only XML file: `pom.xml` (Maven build descriptor)

---

## 5. Containerization & Deployment

### Container Engine
- **Docker** — used only for Oracle Database dependency (not for the application)
- Oracle XE image: `container-registry.oracle.com/database/express:latest`

### Base Image
- **Not Applicable** — no Dockerfile for the application

### Image Size / Layers / Multi-stage Build
- **Not Applicable** — application is not containerized

### Volume Mounts
- **Not Applicable** — no container volume configuration

### Orchestration Tool
- **None** — no Docker Compose, Kubernetes, or Docker Swarm

### Service Definition
- **None** — no formal service definition files

### Network Settings
- **Host network** — application binds to `0.0.0.0:8080` on the host

### Resource Limits
- **None defined** — governed by JVM and host OS defaults

### Health Checks
- **None configured** — no HEALTHCHECK, liveness/readiness probes, or `/actuator/health`

---

## 6. Security

### Security Implementation
- **Authentication**: None
- **Authorization**: None
- **Transport**: HTTP only (no HTTPS/TLS)
- **Encryption**: None
- **Security Headers**: None
- **Spring Security**: Not included
- **Risk**: Hardcoded database credentials (`system/oracle`) in `application.yaml`

### Data Classification
- **Classification**: Internal
- **Data types**: Todo items (title, description, priority, due date, status)
- **PII**: None detected
- **PHI / PCI**: None detected

### Compliance Requirements
- **None detected** — demonstration/sample application with no regulated data

### Licensing
- **Primary License**: MIT (Copyright © Microsoft Corporation)
- **Spring Boot**: Apache License 2.0
- **Oracle JDBC (ojdbc11)**: Oracle Technology Network License (review for commercial use)
- **Lombok**: MIT

---

## 7. Observability & Instrumentation

### Startup Instrumentation
- **Logging**: SLF4J + Logback (Spring Boot default)
- **Usage**: `@Slf4j` in `OracleSqlDemonstrator.java`
- **Output**: Console (default Spring Boot format)
- **Telemetry / APM**: None
- **AOP**: None
- **Metrics (Micrometer)**: Not configured

### Testing Framework
- **Declared**: JUnit 5 (Jupiter), Mockito, AssertJ (via `spring-boot-starter-test`)
- **Test files**: 0 — no tests written

---

## 8. Operating Environment

### Operating System
- **Cross-platform** (Linux / macOS / Windows supported by JVM)
- Oracle XE container runs on Linux

### Hardware Requirements
- **No explicit requirements documented**
- Estimated minimum (Oracle XE 21c): 2GB RAM, 2 CPU cores, 12GB disk

### System Packages
- **Not Applicable** — no container image

### Embedded Language Usage
- **None** — pure Java implementation; Oracle SQL/PL/SQL executed via standard JDBC

---

## 9. Oracle-Specific Features

This application contains Oracle-specific SQL that will require migration work:

| Feature | Location | Migration Impact |
|---------|---------|-----------------|
| `SYSDATE` / `SYSTIMESTAMP` | `TodoService.java` | → `CURRENT_DATE` / `NOW()` in PostgreSQL |
| `ROWNUM` | `TodoRepository.java` | → `LIMIT` clause in PostgreSQL |
| `DBMS_LOB.INSTR()` | `TodoService.java` | → `POSITION()` in PostgreSQL |
| `VARCHAR2` | `schema.sql` | → `VARCHAR` in PostgreSQL |
| PL/SQL blocks | `OracleSqlDemonstrator.java` | → PL/pgSQL in PostgreSQL |
| Oracle JDBC driver (ojdbc11) | `pom.xml` | → PostgreSQL JDBC driver |
| Oracle Hibernate dialect | `application.yaml` | → PostgreSQL dialect |
| `IDENTITY` column | `schema.sql` | → `SERIAL`/`GENERATED` in PostgreSQL |

---

## 10. Migration Readiness Assessment

### Strengths
- Modern Java 17 + Spring Boot 3.2.4 (current LTS versions)
- Standard layered architecture — easy to understand and migrate
- Small codebase (6 source files) — low migration complexity
- Jakarta EE 10 (already migrated from `javax.*` to `jakarta.*`)

### Gaps & Risks
- **No containerization** — requires Dockerfile creation for cloud deployment
- **No environment profiles** — all config hardcoded, needs externalization
- **No health checks** — needed for production Kubernetes/ACA deployment
- **No security** — authentication and HTTPS must be added for production
- **Oracle-specific SQL** — multiple Oracle dialect queries need rewriting for PostgreSQL
- **Hardcoded credentials** — must be moved to environment variables or Key Vault
- **No tests** — migration validation will be challenging without test coverage

