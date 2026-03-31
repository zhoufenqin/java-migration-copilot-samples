# Architecture Diagram

The Asset Manager is a multi-module Spring Boot 2.7 application comprising a web frontend for file upload and viewing, and a background worker for image processing, both sharing AWS S3 storage, RabbitMQ messaging, and a PostgreSQL database.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        Browser["Web Browser"]
    end
    subgraph Web["Web Module - Spring Boot 2.7 / Java 8"]
        MVC["Spring MVC + Thymeleaf"]
        SvcW["Storage Service (Local / S3)"]
        MsgPub["RabbitMQ Message Publisher"]
        MsgSub["BackupMessageProcessor (AMQP Listener)"]
        RepoW["ImageMetadataRepository (JPA)"]
    end
    subgraph Worker["Worker Module - Spring Boot 2.7 / Java 8"]
        MsgCons["RabbitMQ Message Consumer"]
        ProcSvc["FileProcessingService (Local / S3)"]
        RepoWk["ImageMetadataRepository (JPA)"]
    end
    subgraph Data["Data Layer"]
        DB[("PostgreSQL")]
        S3[("AWS S3")]
        RabbitMQ[("RabbitMQ")]
    end

    Browser -->|"HTTP requests"| MVC
    MVC -->|"upload / list / view"| SvcW
    SvcW -->|"store / retrieve files"| S3
    SvcW -->|"persist metadata"| RepoW
    RepoW -->|"SQL queries"| DB
    MVC -->|"trigger backup"| MsgPub
    MsgPub -->|"publish message"| RabbitMQ
    RabbitMQ -->|"deliver message"| MsgSub
    MsgSub -->|"process backup"| SvcW
    RabbitMQ -->|"deliver message"| MsgCons
    MsgCons -->|"thumbnail generation"| ProcSvc
    ProcSvc -->|"read / write files"| S3
    ProcSvc -->|"persist metadata"| RepoWk
    RepoWk -->|"SQL queries"| DB
```

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation (Web)"]
        HomeCtrl["HomeController"]
        S3Ctrl["S3Controller"]
    end
    subgraph BusinessWeb["Business Logic (Web)"]
        StorageSvc["StorageService"]
        LocalStoreSvc["LocalFileStorageService"]
        S3Svc["AwsS3Service"]
        BackupProc["BackupMessageProcessor"]
    end
    subgraph BusinessWorker["Business Logic (Worker)"]
        FileProc["FileProcessor"]
        LocalFileProc["LocalFileProcessingService"]
        S3FileProc["S3FileProcessingService"]
        AbsFileProc["AbstractFileProcessingService"]
    end
    subgraph DataAccess["Data Access"]
        WebRepo["ImageMetadataRepository (Web)"]
        WorkerRepo["ImageMetadataRepository (Worker)"]
    end
    subgraph Infra["Infrastructure / Config"]
        RabbitCfgW["RabbitConfig (Web)"]
        S3CfgW["AwsS3Config (Web)"]
        RabbitCfgWk["RabbitConfig (Worker)"]
        S3CfgWk["AwsS3Config (Worker)"]
        WebMvc["WebMvcConfig"]
        StorageUtil["StorageUtil"]
    end

    HomeCtrl -->|"delegates"| StorageSvc
    S3Ctrl -->|"delegates"| StorageSvc
    StorageSvc -->|"implemented by"| LocalStoreSvc
    StorageSvc -->|"implemented by"| S3Svc
    LocalStoreSvc -->|"queries"| WebRepo
    S3Svc -->|"queries"| WebRepo
    BackupProc -->|"delegates"| StorageSvc
    RabbitCfgW -.->|"configures"| BackupProc
    S3CfgW -.->|"configures"| S3Svc
    WebMvc -.->|"configures"| HomeCtrl
    FileProc -->|"implemented by"| LocalFileProc
    FileProc -->|"implemented by"| S3FileProc
    AbsFileProc -->|"extended by"| LocalFileProc
    AbsFileProc -->|"extended by"| S3FileProc
    S3FileProc -->|"queries"| WorkerRepo
    LocalFileProc -->|"queries"| WorkerRepo
    RabbitCfgWk -.->|"configures"| FileProc
    S3CfgWk -.->|"configures"| S3FileProc
    StorageUtil -.->|"used by"| AbsFileProc
```
