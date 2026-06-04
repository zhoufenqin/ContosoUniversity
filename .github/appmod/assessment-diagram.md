# ContosoUniversity Architecture Diagram

## Application Overview

**Application Name:** ContosoUniversity  
**Type:** ASP.NET Core 6.0 Web Application  
**Framework:** Razor Pages  
**Assessment Date:** 2026-02-07  

## Architecture Diagram

```mermaid
graph TB
    subgraph "Presentation Layer"
        A[Razor Pages UI]
        A1[Students Pages]
        A2[Courses Pages]
        A3[Instructors Pages]
        A4[Departments Pages]
        A5[Static Files wwwroot]
    end
    
    subgraph "Application Layer"
        B[ASP.NET Core 6.0]
        B1[Program.cs]
        B2[Middleware Pipeline]
    end
    
    subgraph "Data Access Layer"
        C[Entity Framework Core]
        C1[SchoolContext DbContext]
        C2[DbInitializer]
    end
    
    subgraph "Domain Models"
        D1[Student]
        D2[Course]
        D3[Enrollment]
        D4[Instructor]
        D5[Department]
        D6[OfficeAssignment]
    end
    
    subgraph "Data Storage"
        E[SQL Server LocalDB]
        E1[SchoolContext Database]
    end
    
    A --> B
    A1 --> B
    A2 --> B
    A3 --> B
    A4 --> B
    B --> C
    C --> C1
    C1 --> D1
    C1 --> D2
    C1 --> D3
    C1 --> D4
    C1 --> D5
    C1 --> D6
    C1 --> E
    C2 --> E
```

## Technology Stack

### Frontend
- **UI Framework:** ASP.NET Core Razor Pages
- **CSS Framework:** Bootstrap 5.x
- **JavaScript:** jQuery with validation plugins

### Backend
- **Runtime:** .NET 6.0
- **Web Framework:** ASP.NET Core 6.0
- **ORM:** Entity Framework Core 6.0.2
- **Database Provider:** Microsoft.EntityFrameworkCore.SqlServer 6.0.2

### Database
- **Type:** SQL Server
- **Instance:** LocalDB (mssqllocaldb)
- **Database:** SchoolContext
- **Connection Management:** Connection strings in appsettings.json

### Development Tools
- **Code Generation:** Microsoft.VisualStudio.Web.CodeGeneration.Design 6.0.2
- **Diagnostics:** Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore 6.0.2
- **Migrations:** Entity Framework Core Migrations

## Application Structure

### Domain Entities
1. **Student** - Student information and enrollments
2. **Course** - Course catalog and details
3. **Enrollment** - Student course registrations with grades
4. **Instructor** - Instructor information and course assignments
5. **Department** - Academic departments
6. **OfficeAssignment** - Instructor office locations

### Relationships
- Students have many Enrollments
- Courses have many Enrollments
- Instructors teach many Courses (many-to-many)
- Courses belong to Departments
- Instructors have one OfficeAssignment

### Key Pages
- **Students:** CRUD operations for student management
- **Courses:** Course management with department association
- **Instructors:** Instructor management with course assignments
- **Departments:** Department administration
- **About:** Student statistics by enrollment date

## Data Flow

```mermaid
sequenceDiagram
    participant User
    participant RazorPages
    participant Controllers
    participant EFCore
    participant SQLServer

    User->>RazorPages: HTTP Request
    RazorPages->>Controllers: Page Handler
    Controllers->>EFCore: LINQ Query
    EFCore->>SQLServer: SQL Command
    SQLServer-->>EFCore: Result Set
    EFCore-->>Controllers: Entity Objects
    Controllers-->>RazorPages: View Model
    RazorPages-->>User: HTML Response
```

## Configuration

### Connection Strings
- **SchoolContext:** SQL Server LocalDB connection with MultipleActiveResultSets enabled

### Application Settings
- **PageSize:** 3 (pagination)
- **Logging:** Information level for application, Warning for ASP.NET Core
- **AllowedHosts:** * (all hosts allowed)

## Migration Readiness

### Assessment Summary
- **Total Issues:** 2
- **Severity Breakdown:**
  - Mandatory: 0
  - Optional: 1 (Static files - consider CDN for scale)
  - Potential: 1 (Connection strings - requires Azure SQL configuration)
  - Information: 0

### Key Migration Considerations
1. **Static Files (Scale.0001):** 33 static files in wwwroot should be served from CDN for better scalability
2. **Connection Strings (Connection.0001):** SQL Server LocalDB connection needs migration to Azure SQL Database

## Architecture Patterns

### Current Patterns
- **Repository Pattern:** Via Entity Framework Core DbContext
- **Code-First Migrations:** Database schema managed through EF Core migrations
- **Dependency Injection:** Built-in ASP.NET Core DI container
- **Configuration Management:** appsettings.json with environment overrides
- **Database Initialization:** Automatic migration and seed data on startup

### Design Characteristics
- **Layered Architecture:** Clear separation between presentation, business logic, and data access
- **Convention over Configuration:** Follows ASP.NET Core Razor Pages conventions
- **ORM-Based Data Access:** Entity Framework Core for database operations
- **Stateless Web Tier:** No session state, suitable for cloud deployment
