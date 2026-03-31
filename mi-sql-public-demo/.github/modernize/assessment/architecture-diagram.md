# Architecture Diagram

This is a standalone Java console application that connects to Azure SQL Database using Managed Identity (MSI) authentication via the Microsoft JDBC driver.

## Application Architecture

```mermaid
flowchart TD
    subgraph App["Application Layer - Java 17"]
        Main["MainSQL - Entry Point"]
        Config["application.properties"]
    end
    subgraph Auth["Authentication Layer"]
        MSI["Azure Managed Identity MSI"]
    end
    subgraph Data["Data Layer"]
        JDBC["Microsoft JDBC Driver mssql-jdbc"]
        DB[("Azure SQL Database")]
    end

    Main -->|"reads config"| Config
    Main -->|"builds connection string"| JDBC
    Config -->|"AZURE_CLIENT_ID"| MSI
    Config -->|"AZURE_SQLDB_CONNECTIONSTRING"| JDBC
    MSI -->|"ActiveDirectoryMSI auth"| JDBC
    JDBC -->|"TCP 1433 encrypted"| DB
```

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Entry Point"]
        MainSQL["MainSQL"]
    end
    subgraph Business["Business Logic"]
        PropLoader["Properties Loader"]
        ConnBuilder["Connection Builder"]
    end
    subgraph DataAccess["Data Access"]
        DSConfig["SQLServerDataSource"]
        SQLConn["SQL Connection"]
    end
    subgraph Infra["Infrastructure"]
        AppProps["application.properties"]
        MSIAuth["MSI Authentication"]
    end

    MainSQL -->|"invokes"| PropLoader
    MainSQL -->|"invokes"| ConnBuilder
    PropLoader -->|"reads"| AppProps
    ConnBuilder -->|"uses clientId"| MSIAuth
    ConnBuilder -->|"configures"| DSConfig
    DSConfig -->|"opens"| SQLConn
    MSIAuth -.->|"authenticates"| SQLConn
```
