# Architecture Diagram

ContosoUniversity is a .NET Framework 4.8 ASP.NET MVC 5 application for university management, using Entity Framework Core 3.1 for data access, SQL Server for persistence, MSMQ for notifications, and local file storage for teaching material uploads.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        Browser["Web Browser"]
    end

    subgraph App["Application Layer - ASP.NET MVC 5 / .NET Framework 4.8"]
        MVC["ASP.NET MVC 5 Controllers + Razor Views"]
        Auth["Windows Authentication"]
        Bundling["Script and Style Bundling"]
        NotifSvc["NotificationService - MSMQ"]
        LogSvc["LoggingService"]
    end

    subgraph Data["Data Access Layer"]
        EF["Entity Framework Core 3.1"]
        DbCtx[("SchoolContext - DbContext")]
        Init["DbInitializer - Seed Data"]
    end

    subgraph Storage["Storage"]
        DB[("SQL Server - LocalDb")]
        Files[("Local File System - Uploads")]
    end

    subgraph External["External Services"]
        MSMQ["Windows Message Queue - MSMQ"]
        WinAuth["Windows Authentication - IIS"]
    end

    Browser -->|"HTTP requests"| MVC
    MVC --> Auth -->|"authorized"| MVC
    MVC -->|"delegates"| NotifSvc
    MVC -->|"CRUD operations"| EF
    EF -->|"queries and commands"| DbCtx
    DbCtx -->|"SQL queries"| DB
    Init -->|"seeds"| DB
    MVC -->|"stores teaching files"| Files
    NotifSvc -->|"sends notifications"| MSMQ
    Auth -->|"validates"| WinAuth
```

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation Layer"]
        HomeCtrl["HomeController"]
        StudCtrl["StudentsController"]
        CourseCtrl["CoursesController"]
        InstrCtrl["InstructorsController"]
        DeptCtrl["DepartmentsController"]
        NotifCtrl["NotificationsController"]
        BaseCtrl["BaseController"]
    end

    subgraph Business["Business Logic"]
        NotifSvc["NotificationService"]
        LogSvc["LoggingService"]
        PagList["PaginatedList"]
    end

    subgraph DataAccess["Data Access"]
        SchoolCtx["SchoolContext"]
        DbInit["DbInitializer"]
        CtxFactory["SchoolContextFactory"]
    end

    subgraph Domain["Domain Models"]
        Person["Person - base"]
        Student["Student"]
        Instructor["Instructor"]
        Course["Course"]
        Dept["Department"]
        Enrollment["Enrollment"]
        Notification["Notification"]
    end

    subgraph Infra["Infrastructure"]
        BundleConf["BundleConfig"]
        RouteConf["RouteConfig"]
        FilterConf["FilterConfig"]
    end

    StudCtrl -->|"extends"| BaseCtrl
    CourseCtrl -->|"extends"| BaseCtrl
    InstrCtrl -->|"extends"| BaseCtrl
    DeptCtrl -->|"extends"| BaseCtrl
    NotifCtrl -->|"extends"| BaseCtrl

    StudCtrl -->|"uses"| SchoolCtx
    CourseCtrl -->|"uses"| SchoolCtx
    InstrCtrl -->|"uses"| SchoolCtx
    DeptCtrl -->|"uses"| SchoolCtx
    NotifCtrl -->|"uses"| SchoolCtx

    StudCtrl -->|"triggers"| NotifSvc
    CourseCtrl -->|"triggers"| NotifSvc
    InstrCtrl -->|"triggers"| NotifSvc

    StudCtrl -->|"paginates"| PagList

    SchoolCtx -->|"manages"| Student
    SchoolCtx -->|"manages"| Instructor
    SchoolCtx -->|"manages"| Course
    SchoolCtx -->|"manages"| Dept
    SchoolCtx -->|"manages"| Enrollment
    SchoolCtx -->|"manages"| Notification

    Student -->|"inherits"| Person
    Instructor -->|"inherits"| Person

    DbInit -->|"seeds"| SchoolCtx
    CtxFactory -->|"creates"| SchoolCtx

    FilterConf -.->|"intercepts"| Presentation
    RouteConf -.->|"routes"| Presentation
    BundleConf -.->|"bundles assets for"| Presentation
```
