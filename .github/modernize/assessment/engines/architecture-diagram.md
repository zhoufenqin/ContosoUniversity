# Architecture Diagram

ContosoUniversity is a single ASP.NET Core Razor Pages application that serves server-rendered academic administration pages and persists data through Entity Framework Core. The solution has one deployable web frontend, one EF Core data access layer, and one SQL Server-backed relational store.

## Application Architecture

```mermaid
flowchart TD
    Browser["Browser user"] -->|"HTTPS requests"| Razor

    subgraph Presentation["Presentation Layer - ASP.NET Core Razor Pages 6.0"]
        Razor["Razor Pages UI"]
        StudentsPages["Students pages"]
        CoursesPages["Courses pages"]
        DepartmentsPages["Departments pages"]
        InstructorsPages["Instructors pages"]
        AboutPage["About reporting page"]
    end

    subgraph Application["Application Layer - PageModel handlers"]
        StudentHandlers["Student CRUD, search, sort, pagination"]
        CourseHandlers["Course CRUD"]
        DepartmentHandlers["Department CRUD and concurrency handling"]
        InstructorHandlers["Instructor CRUD and course assignment"]
        Bootstrap["Startup migration and seeding"]
    end

    subgraph Data["Data Layer - EF Core 6.0.2"]
        SchoolContext["SchoolContext DbContext"]
        ViewModels["View models and pagination helpers"]
        SqlDb[("SQL Server database")]
    end

    StudentsPages -->|"render and submit forms"| StudentHandlers
    CoursesPages -->|"render and submit forms"| CourseHandlers
    DepartmentsPages -->|"render and submit forms"| DepartmentHandlers
    InstructorsPages -->|"render and submit forms"| InstructorHandlers
    AboutPage -->|"request enrollment statistics"| StudentHandlers
    Razor -->|"resolve handlers"| StudentHandlers
    Razor -->|"resolve handlers"| CourseHandlers
    Razor -->|"resolve handlers"| DepartmentHandlers
    Razor -->|"resolve handlers"| InstructorHandlers
    Bootstrap -->|"apply migrations and seed sample data"| SchoolContext
    StudentHandlers -->|"query and update academic records"| SchoolContext
    CourseHandlers -->|"query and update catalog data"| SchoolContext
    DepartmentHandlers -->|"load administrators and save with concurrency token"| SchoolContext
    InstructorHandlers -->|"eager load related data and maintain many-to-many links"| SchoolContext
    StudentHandlers -->|"shape paged and grouped results"| ViewModels
    InstructorHandlers -->|"shape drill-down results"| ViewModels
    SchoolContext -->|"SQL queries and migrations"| SqlDb
```

### Technology Stack Summary

| Layer | Technology | Version | Purpose |
| --- | --- | --- | --- |
| Presentation | ASP.NET Core Razor Pages | 6.0 | Server-rendered web UI for university administration workflows |
| Application | PageModel handlers | 6.0 | Executes CRUD, search, drill-down, and concurrency-aware update flows |
| Data Access | Entity Framework Core with SQL Server provider | 6.0.2 | Maps entities, runs migrations, and issues LINQ-backed SQL queries |
| Data Store | SQL Server / LocalDB connection string | Configured in appsettings | Stores students, instructors, departments, courses, enrollments, and office assignments |
| Bootstrap | EF Core migrations plus DbInitializer | 6.0.2 | Creates schema on startup and seeds baseline academic data |

### Data Storage & External Services

The application uses a single SQL Server-compatible relational database configured through the `SchoolContext` connection string. No message brokers, caches, or third-party HTTP integrations were found; the only external dependency captured in repository metadata is the SQL Server service connection used by EF Core.

### Key Architectural Decisions

- Uses a monolithic Razor Pages design where UI handlers query `SchoolContext` directly instead of introducing a separate service or repository layer.
- Applies EF Core migrations and sample data seeding during startup so a fresh environment becomes usable before the first request is served.
- Handles richer read models with eager loading and helper view models, such as instructor drill-downs and paginated student listings.

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        HomePages["Home, About, Privacy, Error pages"]
        StudentPages["Students PageModels"]
        CoursePages["Courses PageModels"]
        DepartmentPages["Departments PageModels"]
        InstructorPages["Instructors PageModels"]
    end

    subgraph Business["Business Logic"]
        StudentFlow["Search, sort, and paginate students"]
        CourseFlow["Manage course catalog"]
        DepartmentFlow["Maintain departments with optimistic concurrency"]
        InstructorFlow["Assign instructors to courses"]
        ReportingFlow["Aggregate enrollment dates"]
    end

    subgraph DataAccess["Data Access"]
        Context["SchoolContext"]
        Pagination["PaginatedList"]
        InstructorData["InstructorIndexData and AssignedCourseData"]
        EnrollmentStats["EnrollmentDateGroup"]
    end

    subgraph Infra["Infrastructure"]
        Config["Configuration and DI"]
        Seed["DbInitializer"]
        Database[("SQL Server")]
    end

    StudentPages -->|"delegates"| StudentFlow
    CoursePages -->|"delegates"| CourseFlow
    DepartmentPages -->|"delegates"| DepartmentFlow
    InstructorPages -->|"delegates"| InstructorFlow
    HomePages -->|"delegates reporting"| ReportingFlow
    StudentFlow -->|"queries"| Context
    CourseFlow -->|"queries"| Context
    DepartmentFlow -->|"queries and saves"| Context
    InstructorFlow -->|"queries and saves"| Context
    ReportingFlow -->|"groups by enrollment date"| Context
    StudentFlow -->|"creates page slices"| Pagination
    InstructorFlow -->|"builds drill-down models"| InstructorData
    ReportingFlow -->|"projects statistics"| EnrollmentStats
    Config -.->|"configures"| Context
    Seed -.->|"initializes"| Context
    Context -->|"persists"| Database
```

### Component Inventory

| Component | Layer | Type | Responsibility |
| --- | --- | --- | --- |
| `Program` | Infrastructure | Startup | Registers Razor Pages and `SchoolContext`, applies migrations, seeds data, and configures middleware |
| `Pages/Students/*.cshtml.cs` | Presentation | Razor PageModels | Supports student list, details, create, edit, delete, and filtered pagination workflows |
| `Pages/Courses/*.cshtml.cs` | Presentation | Razor PageModels | Maintains course catalog records and department selections |
| `Pages/Departments/*.cshtml.cs` | Presentation | Razor PageModels | Manages department records and handles optimistic concurrency conflicts |
| `Pages/Instructors/*.cshtml.cs` | Presentation | Razor PageModels | Maintains instructors, office assignments, assigned courses, and course enrollment drill-downs |
| `Pages/About.cshtml.cs` | Presentation | Razor PageModel | Produces grouped enrollment statistics for reporting |
| `SchoolContext` | Data Access | EF Core DbContext | Exposes entity sets and configures table mappings and the instructor-course many-to-many relationship |
| `PaginatedList<T>` | Data Access | Helper | Creates paged student result sets from LINQ queries |
| `InstructorIndexData`, `AssignedCourseData`, `EnrollmentDateGroup` | Data Access | View models | Shape related entity data for instructor and reporting views |
| `DbInitializer` | Infrastructure | Seed component | Inserts baseline students, departments, courses, instructors, enrollments, and office assignments |
