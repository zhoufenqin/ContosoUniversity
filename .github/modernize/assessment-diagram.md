# ContosoUniversity Architecture Diagram

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        Browser["Web Browser"]
    end

    subgraph Presentation["Presentation Layer (ASP.NET Core 6 Razor Pages)"]
        RazorPages["Razor Pages\n(Students, Courses, Departments, Instructors)"]
        StaticFiles["Static Files\n(wwwroot: CSS, JS, Images)"]
    end

    subgraph Business["Business Logic Layer"]
        PaginatedList["PaginatedList"]
        DbInitializer["DbInitializer"]
    end

    subgraph DataAccess["Data Access Layer (Entity Framework Core 6)"]
        SchoolContext["SchoolContext\n(DbContext)"]
        Models["Domain Models\n(Student, Course, Enrollment,\nInstructor, Department,\nOfficeAssignment)"]
        Migrations["EF Core Migrations"]
    end

    subgraph Storage["Data Storage"]
        SQLServer["SQL Server\n(localdb / mssqllocaldb)"]
    end

    Browser -->|"HTTPS requests"| RazorPages
    Browser -->|"Static assets"| StaticFiles
    RazorPages --> PaginatedList
    RazorPages --> SchoolContext
    DbInitializer --> SchoolContext
    SchoolContext --> Models
    SchoolContext --> Migrations
    SchoolContext -->|"SQL queries"| SQLServer
```

## Technology Stack

| Layer | Technology |
|-------|-----------|
| Framework | ASP.NET Core 6 (net6.0) |
| UI | Razor Pages |
| ORM | Entity Framework Core 6 |
| Database | SQL Server (localdb) |
| Language | C# |

## Assessment Summary

Based on AppCAT analysis, **2 issues** were identified:

| Category | Severity | Description |
|----------|----------|-------------|
| Scale | Optional | Static content served from application (wwwroot) — consider Azure CDN or Azure Blob Storage for scalability |
| Connection | Potential | SQL Server connection string in appsettings.json — migrate to Azure SQL Database with managed identity or Azure Key Vault |

## Migration Considerations

- **Static Content**: Move static assets (CSS, JS, images) to Azure Blob Storage with Azure CDN for improved performance and scalability
- **Database**: Migrate from SQL Server localdb to **Azure SQL Database**; use managed identity instead of connection strings in config files
- **Hosting**: Application is compatible with Azure App Service (Windows/Linux), Azure Kubernetes Service, and Azure Container Apps
