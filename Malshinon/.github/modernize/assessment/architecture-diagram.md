# Architecture Diagram

Malshinon is a .NET Framework 4.8 console application that manages intelligence reports between agents and targets, using a MySQL database for persistence and a factory/DAL pattern for data operations.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        CLI["Console Menu UI"]
    end
    subgraph App["Application Layer - .NET Framework 4.8"]
        Factory["Factory Layer\n(PeopleFactory, ReportFactory)"]
        DAL["Data Access Layer\n(PeopleDAL, ReportDAL)"]
        Util["Utilities\n(Logger, CSV Importer)"]
    end
    subgraph Data["Data Layer"]
        DB[("MySQL Database\n(malshinon)")]
        LogFile[("Log File\n(log.txt)")]
        CSVFile[("CSV Files\n(Bulk Import)")]
    end

    CLI -->|"user actions"| Factory
    CLI -->|"user actions"| DAL
    CLI -->|"view logs"| Util
    CLI -->|"import CSV"| Util
    Factory -->|"creates entities"| DAL
    DAL -->|"raw SQL"| DB
    Util -->|"writes"| LogFile
    Util -->|"reads"| CSVFile
    Util -->|"inserts via DAL"| DAL
```

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        Program["Program\n(Console UI)"]
    end
    subgraph Business["Business Logic"]
        PeopleFactory["PeopleFactory"]
        ReportFactory["ReportFactory"]
        CSV["CSV Importer"]
        Logger["Logger"]
    end
    subgraph DataAccess["Data Access"]
        PeopleDAL["PeopleDAL"]
        ReportDAL["ReportDAL"]
        DBConn["DBConnection"]
    end
    subgraph Models["Models"]
        People["People"]
        Report["Report"]
    end

    Program -->|"creates people"| PeopleFactory
    Program -->|"creates reports"| ReportFactory
    Program -->|"imports"| CSV
    Program -->|"reads logs"| Logger
    Program -->|"queries"| PeopleDAL
    Program -->|"queries"| ReportDAL
    PeopleFactory -->|"builds"| People
    ReportFactory -->|"builds"| Report
    People -->|"persists via"| PeopleDAL
    Report -->|"persists via"| ReportDAL
    CSV -->|"delegates"| PeopleDAL
    CSV -->|"delegates"| ReportDAL
    PeopleDAL -->|"executes SQL"| DBConn
    ReportDAL -->|"executes SQL"| DBConn
    ReportDAL -->|"triggers checks"| PeopleDAL
```
