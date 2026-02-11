# ContosoUniversity Application Architecture

## Overview

ContosoUniversity is an ASP.NET Core 6.0 web application built using Razor Pages architecture, demonstrating a university management system with Entity Framework Core for data access.

## Architecture Diagram

```mermaid
graph TB
    subgraph "Presentation Layer"
        A[Razor Pages UI]
        A1[Pages/Students]
        A2[Pages/Courses]
        A3[Pages/Instructors]
        A4[Pages/Departments]
        A5[Static Files - wwwroot]
    end

    subgraph "Application Layer"
        B[ASP.NET Core 6.0]
        B1[Razor Pages Framework]
        B2[Model Binding & Validation]
        B3[Routing & Middleware]
    end

    subgraph "Data Access Layer"
        C[Entity Framework Core 6.0]
        C1[SchoolContext - DbContext]
        C2[DbInitializer]
        C3[EF Migrations]
    end

    subgraph "Domain Models"
        D[Models]
        D1[Student]
        D2[Course]
        D3[Enrollment]
        D4[Instructor]
        D5[Department]
        D6[OfficeAssignment]
    end

    subgraph "Data Storage"
        E[(SQL Server LocalDB)]
        E1[Students Table]
        E2[Courses Table]
        E3[Enrollments Table]
        E4[Instructors Table]
        E5[Departments Table]
    end

    subgraph "Client-Side Libraries"
        F[Frontend Assets]
        F1[Bootstrap 5]
        F2[jQuery]
        F3[jQuery Validation]
    end

    A --> B
    A1 --> B1
    A2 --> B1
    A3 --> B1
    A4 --> B1
    A5 --> F

    B --> C
    B1 --> C1
    B2 --> D
    B3 --> B1

    C --> D
    C1 --> D1
    C1 --> D2
    C1 --> D3
    C1 --> D4
    C1 --> D5
    C1 --> D6
    C2 --> C1
    C3 --> E

    C1 --> E
    D1 --> E1
    D2 --> E2
    D3 --> E3
    D4 --> E4
    D5 --> E5

    F1 --> A
    F2 --> A
    F3 --> A
```

## Component Details

### Presentation Layer
- **Razor Pages**: Server-side rendered pages for CRUD operations
  - Student management pages
  - Course management pages
  - Instructor management pages
  - Department management pages
  - Home and About pages
- **Static Files**: CSS, JavaScript, and frontend libraries served from wwwroot

### Application Layer
- **ASP.NET Core 6.0**: Web framework providing:
  - Razor Pages framework for page-based development
  - Model binding and validation
  - Routing and middleware pipeline
  - Dependency injection
  - Configuration management

### Data Access Layer
- **Entity Framework Core 6.0**: ORM for database operations
  - SchoolContext: Main DbContext managing all entities
  - DbInitializer: Seeds initial data
  - Code-First Migrations: Database schema versioning
  - LINQ query support

### Domain Models
Six core entities representing the university domain:
- **Student**: Student information and enrollments
- **Course**: Course details and relationships
- **Enrollment**: Student-Course many-to-many relationship
- **Instructor**: Instructor information
- **Department**: Academic departments
- **OfficeAssignment**: Instructor office assignments

### Data Storage
- **SQL Server LocalDB**: Development database
  - Connection managed through appsettings.json
  - Schema created and managed via EF Core migrations
  - Supports multiple active result sets

### Client-Side Libraries
- **Bootstrap 5**: Responsive UI framework
- **jQuery**: DOM manipulation and AJAX
- **jQuery Validation**: Client-side form validation

## Technology Stack Summary

| Layer | Technology | Version |
|-------|-----------|---------|
| Framework | ASP.NET Core | 6.0 |
| UI Pattern | Razor Pages | 6.0 |
| ORM | Entity Framework Core | 6.0 |
| Database | SQL Server | LocalDB |
| CSS Framework | Bootstrap | 5.x |
| JavaScript | jQuery | 3.x |
| Build | MSBuild | .NET SDK |

## Data Flow

1. **User Request**: Browser sends HTTP request to ASP.NET Core
2. **Routing**: Request routed to appropriate Razor Page
3. **Model Binding**: Page model binds request data
4. **Business Logic**: Page handlers execute business operations
5. **Data Access**: EF Core translates LINQ to SQL queries
6. **Database**: SQL Server executes queries and returns data
7. **Response**: Razor engine renders HTML with data
8. **Client**: Browser receives and displays HTML response

## Key Architecture Patterns

- **Repository Pattern**: Implemented via Entity Framework Core DbContext
- **Unit of Work**: DbContext manages transactions and change tracking
- **Dependency Injection**: Services registered in Program.cs
- **Code-First Development**: Database schema defined via C# models
- **Page-Based Architecture**: Self-contained page models with handlers

## Configuration

- **appsettings.json**: Application configuration
  - Connection strings
  - Logging configuration
  - Page size settings
- **Program.cs**: Application startup and service configuration
  - Database context registration
  - Migration execution on startup
  - Middleware pipeline configuration

## Assessment Insights

Based on the AppCAT assessment report:
- **Total Issues**: 2 detected
- **Story Points**: 6 effort
- **Severity**: 1 optional, 1 potential
- **Categories**: Scale, Connection
- **Primary Issue**: Static files in wwwroot (33 files) may need CDN consideration for scale
- **Migration Targets**: Compatible with Azure App Service, AKS, ACA, Container Apps
