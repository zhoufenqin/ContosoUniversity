# ContosoUniversity Architecture Diagram

## Application Overview

ContosoUniversity is an ASP.NET Core 6.0 web application built using Razor Pages that manages university data including students, courses, departments, and instructors.

## Architecture Diagram

```mermaid
graph TB
    subgraph "Presentation Layer"
        A[Razor Pages UI]
        A1[Students Pages]
        A2[Courses Pages]
        A3[Instructors Pages]
        A4[Departments Pages]
        A5[About Page]
    end
    
    subgraph "Application Layer"
        B[ASP.NET Core 6.0 Web Application]
        B1[Program.cs - Application Entry]
        B2[Middleware Pipeline]
        B3[Routing]
    end
    
    subgraph "Data Access Layer"
        C[Entity Framework Core 6.0]
        C1[SchoolContext - DbContext]
        C2[DbInitializer]
        C3[Migrations]
    end
    
    subgraph "Domain Models"
        D[Domain Entities]
        D1[Student]
        D2[Course]
        D3[Instructor]
        D4[Department]
        D5[Enrollment]
        D6[OfficeAssignment]
    end
    
    subgraph "Data Storage"
        E[(SQL Server LocalDB)]
        E1[SchoolContext Database]
    end
    
    A --> B
    A1 --> B
    A2 --> B
    A3 --> B
    A4 --> B
    A5 --> B
    
    B --> C
    B1 --> B2
    B2 --> B3
    B3 --> C
    
    C --> D
    C1 --> D
    C2 --> D
    C3 --> E
    
    D1 --> C1
    D2 --> C1
    D3 --> C1
    D4 --> C1
    D5 --> C1
    D6 --> C1
    
    C1 --> E
    E1 --> E
    
    style A fill:#e1f5ff
    style B fill:#fff4e1
    style C fill:#e8f5e9
    style D fill:#f3e5f5
    style E fill:#ffebee
```

## Technology Stack

### Frontend
- **Framework**: ASP.NET Core Razor Pages
- **Static Files**: wwwroot directory
- **View Engine**: Razor (.cshtml)

### Backend
- **Runtime**: .NET 6.0
- **Framework**: ASP.NET Core 6.0 Web SDK
- **Architecture Pattern**: Razor Pages with Entity Framework Core

### Data Access
- **ORM**: Entity Framework Core 6.0.2
- **Provider**: Microsoft.EntityFrameworkCore.SqlServer
- **Migration Tool**: EF Core Migrations

### Database
- **Database**: SQL Server LocalDB
- **Connection**: Trusted Connection with MultipleActiveResultSets
- **Database Name**: SchoolContext-a8778b0f-1bfd-4d0f-a500-09390a0df97f

### Development Tools
- **Code Generation**: Microsoft.VisualStudio.Web.CodeGeneration.Design 6.0.2
- **Diagnostics**: Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore 6.0.2

## Application Features

### Core Entities
1. **Student** - Manages student information
2. **Course** - Course catalog and details
3. **Instructor** - Faculty and instructor data
4. **Department** - Academic departments
5. **Enrollment** - Student course enrollments
6. **OfficeAssignment** - Instructor office assignments

### Key Functionality
- CRUD operations for Students, Courses, Instructors, and Departments
- Enrollment management
- Database seeding through DbInitializer
- Automatic database migration on application startup
- Pagination support (PageSize: 3)

## Data Flow

1. **User Request** → Razor Pages (UI Layer)
2. **Page Model** → Entity Framework Core (Data Access)
3. **DbContext** → SQL Server LocalDB (Database)
4. **Response** → Razor View → User

## Configuration

- **Logging**: Configured for Information level (Microsoft.AspNetCore at Warning)
- **HTTPS**: Enabled with redirection
- **Static Files**: Enabled for wwwroot
- **Development Mode**: 
  - Developer exception page
  - Migrations endpoint
  - Database developer page exception filter
- **Production Mode**:
  - Exception handler at /Error
  - HSTS enabled

## Architecture Characteristics

- **Pattern**: Three-tier architecture (Presentation, Business/Data Access, Database)
- **Data Access**: Repository pattern via Entity Framework Core
- **UI Pattern**: Razor Pages (Page-based MVC variant)
- **Configuration**: appsettings.json with environment-specific overrides
- **Database Strategy**: Code-first with migrations
