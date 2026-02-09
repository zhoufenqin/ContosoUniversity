# ContosoUniversity Architecture Diagram

## Application Overview

**Application Type**: ASP.NET Core 6.0 Razor Pages Web Application  
**Target Framework**: .NET 6.0  
**Architecture Pattern**: 3-Tier Web Application

## Architecture Diagram

```mermaid
graph TB
    subgraph "Presentation Layer"
        A[Razor Pages UI]
        A1[Students Pages]
        A2[Courses Pages]
        A3[Departments Pages]
        A4[Instructors Pages]
        A5[About/Home Pages]
        
        A --> A1
        A --> A2
        A --> A3
        A --> A4
        A --> A5
    end
    
    subgraph "Application Layer"
        B[ASP.NET Core 6.0]
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
        C[Entity Framework Core 6.0.2]
        C1[SchoolContext DbContext]
        C2[DbInitializer]
        C3[Migrations]
        
        C --> C1
        C --> C2
        C --> C3
    end
    
    subgraph "Domain Models"
        D[Domain Entities]
        D1[Student]
        D2[Course]
        D3[Enrollment]
        D4[Department]
        D5[Instructor]
        D6[OfficeAssignment]
        
        D --> D1
        D --> D2
        D --> D3
        D --> D4
        D --> D5
        D --> D6
    end
    
    subgraph "Data Storage"
        E[SQL Server LocalDB]
        E1[SchoolContext Database]
        
        E --> E1
    end
    
    A1 -->|CRUD Operations| C1
    A2 -->|CRUD Operations| C1
    A3 -->|CRUD Operations| C1
    A4 -->|CRUD Operations| C1
    A5 -->|Statistics Query| C1
    
    B1 -->|Request Processing| A
    B2 -->|Route Mapping| A
    
    C1 -->|Entity Mapping| D
    C2 -->|Seed Data| E1
    C3 -->|Schema Updates| E1
    C1 -->|SQL Queries| E1
    
    style A fill:#e1f5ff
    style B fill:#fff4e1
    style C fill:#e8f5e9
    style D fill:#f3e5f5
    style E fill:#ffebee
```

## Technology Stack

### Web Framework
- **ASP.NET Core**: 6.0
- **UI Technology**: Razor Pages
- **Authentication**: Built-in ASP.NET Core Authorization

### Data Access
- **ORM**: Entity Framework Core 6.0.2
- **Database Provider**: Microsoft.EntityFrameworkCore.SqlServer 6.0.2
- **Migrations**: Entity Framework Core Migrations

### Database
- **Database Engine**: SQL Server (LocalDB)
- **Connection**: Trusted Connection with MultipleActiveResultSets

### Development Tools
- **Diagnostics**: Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore 6.0.2
- **Code Generation**: Microsoft.VisualStudio.Web.CodeGeneration.Design 6.0.2

## Application Components

### Presentation Layer (Razor Pages)
- **Students**: Create, Read, Update, Delete (CRUD) operations for students
- **Courses**: Manage courses and course assignments
- **Departments**: Department management
- **Instructors**: Instructor information and assignments
- **About**: Statistics and enrollment data visualization
- **Static Content**: CSS, JavaScript, images served via wwwroot

### Business Logic
- Embedded in Razor Page Models (PageModel classes)
- PaginatedList utility for pagination support
- DbInitializer for data seeding

### Data Access Layer
- **SchoolContext**: Main DbContext with DbSets for all entities
- **Entity Relationships**:
  - Students enrolled in Courses (via Enrollment)
  - Courses belong to Departments
  - Instructors assigned to Courses (many-to-many)
  - Instructors have OfficeAssignments (one-to-one)

### Domain Models
- **Student**: Student information and enrollments
- **Course**: Course details with credits and department
- **Enrollment**: Student enrollment in courses with grades
- **Department**: Academic departments with budget
- **Instructor**: Instructor details and hire date
- **OfficeAssignment**: Office location for instructors

## Configuration

### Application Settings
- **PageSize**: 3 (pagination)
- **Logging**: Information level for default, Warning for ASP.NET Core
- **AllowedHosts**: * (all hosts)

### Connection Strings
- **SchoolContext**: SQL Server LocalDB connection with trusted authentication

## Data Flow

1. **User Request**: Browser sends HTTP request to ASP.NET Core application
2. **Routing**: Middleware pipeline routes request to appropriate Razor Page
3. **Page Processing**: PageModel processes request, interacts with SchoolContext
4. **Data Access**: Entity Framework Core translates LINQ queries to SQL
5. **Database Query**: SQL Server executes queries and returns results
6. **Entity Mapping**: EF Core maps results to domain entities
7. **Response Rendering**: Razor Page renders HTML with data
8. **HTTP Response**: Response sent back to browser

## Assessment Findings

Based on the AppCAT assessment report:

- **Issues Found**: 2
- **Incidents**: 2
- **Story Points**: 6
- **Severity**:
  - Optional: 1 issue
  - Potential: 1 issue
- **Categories**:
  - Scale: 1 issue
  - Connection: 1 issue

These issues are related to cloud migration readiness and may require attention when modernizing to Azure.
