# Dependency Map

ContosoUniversity declares 47 NuGet packages spanning web framework, data access, security, caching, logging, and utility libraries for a .NET Framework 4.8 ASP.NET MVC 5 application.

## Dependencies

```mermaid
flowchart LR
    App["ContosoUniversity"]

    subgraph Web["Web Frameworks"]
        AspNetMvc["ASP.NET MVC v5.2.9"]
        AspNetRazor["ASP.NET Razor v3.2.9"]
        WebOptimization["Web.Optimization v1.1.3"]
        Bootstrap["Bootstrap v5.3.3"]
        jQuery["jQuery v3.7.1"]
        jQueryVal["jQuery Validation v1.21.0"]
        Modernizr["Modernizr v2.6.2"]
    end

    subgraph DB["Database and ORM"]
        EFCore["EF Core v3.1.32"]
        EFCoreSql["EF Core SqlServer v3.1.32"]
        SqlClient["SqlClient v2.1.4"]
    end

    subgraph Sec["Security"]
        IdentityClient["Microsoft.Identity.Client v4.21.1"]
    end

    subgraph Cache["Caching"]
        MemCache["Extensions.Caching.Memory v3.1.32"]
    end

    subgraph Log["Logging"]
        ExtLogging["Extensions.Logging v3.1.32"]
        DiagSource["DiagnosticSource v4.7.1"]
    end

    subgraph Infra["Infrastructure"]
        ExtDI["Extensions.DependencyInjection v3.1.32"]
        ExtConfig["Extensions.Configuration v3.1.32"]
        ExtOptions["Extensions.Options v3.1.32"]
        Compiler["CodeDom.DotNetCompilerPlatform v2.0.1"]
    end

    subgraph Util["Utilities"]
        NewtonsoftJson["Newtonsoft.Json v13.0.3"]
        SysAnnotations["ComponentModel.Annotations v4.7.0"]
        SysLibs["System Runtime Libraries v4.x"]
    end

    App -->|"web"| Web
    App -->|"persistence"| DB
    App -->|"security"| Sec
    App -->|"caching"| Cache
    App -->|"logging"| Log
    App -->|"infrastructure"| Infra
    App -->|"utilities"| Util

    EFCore -.->|"uses"| ExtDI
    EFCore -.->|"uses"| ExtLogging
    EFCoreSql -.->|"uses"| SqlClient
    MemCache -.->|"uses"| ExtDI
```
