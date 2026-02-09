# ContosoUniversity Architecture Diagram

## Application Overview

**Application Type:** ASP.NET Core 6.0 Web Application (Razor Pages)  
**Framework:** .NET 6.0  
**Pattern:** MVC-style with Razor Pages  
**Database:** SQL Server (LocalDB)

## Architecture Diagram

```mermaid
graph TB
    subgraph "Client Layer"
        Browser[Web Browser]
    end
    
    subgraph "Presentation Layer"
        Pages[Razor Pages]
        Pages --> Students[Student Pages]
        Pages --> Courses[Course Pages]
        Pages --> Instructors[Instructor Pages]
        Pages --> Departments[Department Pages]
        Pages --> About[About/Reports]
    end
    
    subgraph "Application Layer"
        Program[Program.cs - Entry Point]
        Utilities[Utility Classes]
        Pagination[PaginatedList]
    end
    
    subgraph "Data Access Layer"
        EFCore[Entity Framework Core 6.0]
        SchoolContext[SchoolContext - DbContext]
        DbInitializer[Database Initializer]
    end
    
    subgraph "Domain Models"
        Student[Student Model]
        Course[Course Model]
        Enrollment[Enrollment Model]
        Instructor[Instructor Model]
        Department[Department Model]
        Office[OfficeAssignment Model]
    end
    
    subgraph "Data Storage"
        SQLDB[(SQL Server LocalDB)]
    end
    
    subgraph "Static Resources"
        wwwroot[wwwroot - Static Files]
        Bootstrap[Bootstrap CSS/JS]
        jQuery[jQuery Libraries]
        Validation[Validation Scripts]
    end
    
    Browser -->|HTTP Requests| Pages
    Pages -->|Uses| Utilities
    Pages -->|Uses| Pagination
    Pages -->|Data Operations| SchoolContext
    Program -->|Configures| SchoolContext
    Program -->|Initializes| DbInitializer
    SchoolContext -->|Manages| Student
    SchoolContext -->|Manages| Course
    SchoolContext -->|Manages| Enrollment
    SchoolContext -->|Manages| Instructor
    SchoolContext -->|Manages| Department
    SchoolContext -->|Manages| Office
    SchoolContext -->|Uses| EFCore
    EFCore -->|Queries/Updates| SQLDB
    DbInitializer -->|Seeds Data| SQLDB
    Pages -->|Serves| wwwroot
    wwwroot --> Bootstrap
    wwwroot --> jQuery
    wwwroot --> Validation
    
    style Browser fill:#e1f5ff
    style Pages fill:#fff4e6
    style SchoolContext fill:#e8f5e9
    style SQLDB fill:#f3e5f5
    style wwwroot fill:#fce4ec
```

## Technology Stack

### Frontend
- **UI Framework:** ASP.NET Core Razor Pages
- **CSS Framework:** Bootstrap 5.x
- **JavaScript Libraries:** 
  - jQuery 3.x
  - jQuery Validation
  - jQuery Validation Unobtrusive

### Backend
- **Runtime:** .NET 6.0
- **Web Framework:** ASP.NET Core 6.0 (Microsoft.NET.Sdk.Web)
- **ORM:** Entity Framework Core 6.0.2
- **Database Provider:** Microsoft.EntityFrameworkCore.SqlServer 6.0.2

### Data Storage
- **Database:** SQL Server (LocalDB)
- **Connection:** Trusted Connection with MultipleActiveResultSets
- **Database Name:** SchoolContext

### Development Tools
- **EF Core Tools:** Microsoft.EntityFrameworkCore.Tools 6.0.2
- **Diagnostics:** Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore 6.0.2
- **Code Generation:** Microsoft.VisualStudio.Web.CodeGeneration.Design 6.0.2

## Application Structure

### Data Layer
- **SchoolContext.cs**: Main DbContext managing all entities
- **DbInitializer.cs**: Database seeding and initialization logic
- **Migrations**: EF Core database migrations for schema management

### Domain Models
The application manages a university domain with the following entities:
- **Student**: Student information and enrollments
- **Course**: Course catalog and details
- **Enrollment**: Student-Course enrollment relationship
- **Instructor**: Instructor information and course assignments
- **Department**: Department structure and management
- **OfficeAssignment**: Instructor office assignments

### Presentation Layer
Razor Pages organized by feature:
- **Students/**: Student CRUD operations
- **Courses/**: Course management
- **Instructors/**: Instructor management
- **Departments/**: Department administration
- **About.cshtml**: Analytics and reporting
- **Index.cshtml**: Home page
- **Shared/**: Shared layouts and components

### Utility Classes
- **PaginatedList.cs**: Generic pagination support
- **Utility.cs**: Common helper functions
- **SchoolViewModels**: View models for complex queries

## Key Patterns and Practices

1. **Repository Pattern**: Implemented through EF Core DbContext
2. **Pagination**: Custom PaginatedList for large datasets
3. **Code-First Migrations**: EF Core migrations for database schema
4. **Dependency Injection**: Built-in ASP.NET Core DI container
5. **Configuration Management**: appsettings.json for configuration
6. **Static File Serving**: wwwroot directory for client-side assets

## Database Schema

The application uses Entity Framework Core with the following main tables:
- Course
- Student  
- Instructor
- Enrollment (junction table)
- Department
- OfficeAssignment

Relationships include:
- Many-to-Many: Students ↔ Courses (through Enrollments)
- Many-to-Many: Instructors ↔ Courses
- One-to-One: Instructor ↔ OfficeAssignment
- One-to-Many: Department ↔ Courses

## Assessment Findings

Based on AppCAT analysis (2 issues discovered):

1. **Static Content Management** (Optional - 3 story points)
   - 33 static files in wwwroot detected
   - Consider using Azure CDN or Azure Storage for static content in production
   
2. **Connection String Configuration** (Potential issue)
   - LocalDB connection string detected
   - Needs migration to Azure SQL Database for cloud deployment

## Cloud Migration Considerations

For Azure migration, consider:
- Migrate from LocalDB to Azure SQL Database
- Move static assets (CSS, JS) to Azure CDN or Storage
- Configure connection strings using Azure Key Vault
- Enable Application Insights for monitoring
- Consider Azure App Service for hosting
