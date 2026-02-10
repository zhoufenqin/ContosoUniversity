# ContosoUniversity Architecture Diagram

## Overview

ContosoUniversity is an ASP.NET Core 6.0 web application that demonstrates a university management system using Razor Pages and Entity Framework Core.

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
        A --> A1
        A --> A2
        A --> A3
        A --> A4
        A --> A5
    end

    subgraph "Application Layer"
        B[ASP.NET Core 6.0 Runtime]
        B1[Middleware Pipeline]
        B2[Routing]
        B3[Authorization]
        B4[Static Files]
        B --> B1
        B --> B2
        B --> B3
        B --> B4
    end

    subgraph "Data Access Layer"
        C[Entity Framework Core 6.0]
        C1[SchoolContext]
        C2[Migrations]
        C3[DbInitializer]
        C --> C1
        C --> C2
        C --> C3
    end

    subgraph "Domain Models"
        D1[Student]
        D2[Course]
        D3[Instructor]
        D4[Department]
        D5[Enrollment]
        D6[OfficeAssignment]
    end

    subgraph "Data Storage"
        E[SQL Server LocalDB]
    end

    A -->|HTTP Requests| B
    B -->|Page Models| A
    A1 -->|Uses| C1
    A2 -->|Uses| C1
    A3 -->|Uses| C1
    A4 -->|Uses| C1
    A5 -->|Uses| C1
    C1 -->|Manages| D1
    C1 -->|Manages| D2
    C1 -->|Manages| D3
    C1 -->|Manages| D4
    C1 -->|Manages| D5
    C1 -->|Manages| D6
    C -->|ADO.NET Provider| E
    C2 -->|Schema Updates| E
    C3 -->|Seed Data| E
```

## Technology Stack

### Framework & Runtime
- **ASP.NET Core 6.0**: Web application framework
- **.NET 6.0**: Runtime platform
- **Razor Pages**: UI presentation model

### Data Access
- **Entity Framework Core 6.0.2**: ORM for data access
- **SQL Server Provider**: Database connectivity
- **EF Core Tools**: Development-time tooling
- **Migrations**: Database schema versioning

### Database
- **SQL Server LocalDB**: Development database
- **Connection String**: Configured in appsettings.json
- **Database Name**: SchoolContext

### Development Tools
- **Entity Framework Diagnostics**: Developer exception page
- **Code Generation**: Scaffolding support
- **Migration Endpoint**: Database migration UI

## Application Layers

### 1. Presentation Layer (Razor Pages)
- **Students**: CRUD operations for student management
- **Courses**: Course catalog and management
- **Instructors**: Instructor profiles and assignments
- **Departments**: Department organization
- **About**: Statistics and analytics
- **Shared Components**: Layout, navigation, error pages

### 2. Application Layer (ASP.NET Core)
- **Middleware Pipeline**: Request/response processing
- **Routing**: URL mapping to pages
- **Static Files**: CSS, JavaScript, images
- **HTTPS Redirection**: Security enforcement
- **Developer Exception Page**: Error handling (development)
- **HSTS**: HTTP Strict Transport Security (production)

### 3. Data Access Layer (Entity Framework Core)
- **SchoolContext**: Main DbContext managing all entities
- **Migrations**: Database schema versioning (EF Core Migrations)
- **DbInitializer**: Database seeding with sample data

### 4. Domain Model
- **Student**: Student information and enrollments
- **Course**: Course details and relationships
- **Instructor**: Instructor data and course assignments
- **Department**: Department organization
- **Enrollment**: Student-Course relationships with grades
- **OfficeAssignment**: Instructor office assignments

### 5. Data Storage
- **SQL Server LocalDB**: Relational database for persistence
- **SchoolContext Database**: Main application database

## Key Features

### Database Initialization
- Automatic migration on startup
- Seed data initialization via DbInitializer
- Database created if it doesn't exist

### Entity Relationships
- Many-to-Many: Courses ↔ Instructors
- One-to-Many: Students → Enrollments ← Courses
- One-to-One: Instructor → OfficeAssignment
- One-to-Many: Department → Courses

### Configuration
- **Connection Strings**: Managed in appsettings.json
- **Page Size**: Configurable pagination (default: 3)
- **Logging**: Configured for different environments
- **HTTPS**: Required in production

## Deployment Considerations

### Current Setup
- Development environment uses LocalDB
- Built on .NET 6.0 (LTS version)
- Uses Entity Framework Core 6.0.2

### Assessment Findings
Based on the AppCAT assessment, 2 issues were identified:
- **Story Points**: 6
- **Optional Issues**: 1
- **Potential Issues**: 1

For detailed assessment results, see `report.json` in this directory.

## Next Steps

1. Review the detailed assessment report (`report.json`)
2. Address identified issues before cloud migration
3. Consider upgrading to .NET 8.0 (latest LTS)
4. Plan database migration from LocalDB to Azure SQL Database
5. Review security configurations for cloud deployment
