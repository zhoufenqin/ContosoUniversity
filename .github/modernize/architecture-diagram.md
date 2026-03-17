# Architecture Diagram

ContosoUniversity is an ASP.NET Core 6.0 Razor Pages web application for managing university students, courses, instructors, and departments, using Entity Framework Core with SQL Server for data persistence.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        Browser["Web Browser"]
    end
    subgraph App["Application Layer - ASP.NET Core 6.0 Razor Pages"]
        Pages["Razor Pages (Students / Courses / Departments / Instructors)"]
        PageModels["PageModels (OnGet / OnPost Handlers)"]
        Utilities["PaginatedList / ViewModels"]
    end
    subgraph Data["Data Layer"]
        EF["Entity Framework Core 6.0"]
        DB[("SQL Server LocalDB")]
        DbInit["DbInitializer (Seed Data)"]
    end

    Browser -->|"HTTP requests"| Pages
    Pages -->|"invokes"| PageModels
    PageModels -->|"uses"| Utilities
    PageModels -->|"queries / commands"| EF
    EF -->|"SQL queries"| DB
    DbInit -->|"seeds on startup"| DB
```

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        StudPages["Student Pages"]
        CoursePages["Course Pages"]
        DeptPages["Department Pages"]
        InstrPages["Instructor Pages"]
        OtherPages["Home / About / Error Pages"]
    end
    subgraph Business["Business Logic"]
        StudPM["Student PageModels"]
        CoursePM["Course PageModels"]
        DeptPM["Department PageModels"]
        InstrPM["Instructor PageModels"]
        PaginatedList["PaginatedList"]
        ViewModels["SchoolViewModels"]
    end
    subgraph DataAccess["Data Access"]
        SchoolCtx["SchoolContext (DbContext)"]
        DbInit["DbInitializer"]
    end
    subgraph Entities["Domain Models"]
        Student["Student"]
        Course["Course"]
        Instructor["Instructor"]
        Department["Department"]
        Enrollment["Enrollment"]
        OfficeAssignment["OfficeAssignment"]
    end

    StudPages -->|"backed by"| StudPM
    CoursePages -->|"backed by"| CoursePM
    DeptPages -->|"backed by"| DeptPM
    InstrPages -->|"backed by"| InstrPM
    StudPM -->|"uses"| PaginatedList
    InstrPM -->|"uses"| ViewModels
    CoursePM -->|"uses"| ViewModels
    StudPM -->|"queries"| SchoolCtx
    CoursePM -->|"queries"| SchoolCtx
    DeptPM -->|"queries"| SchoolCtx
    InstrPM -->|"queries"| SchoolCtx
    DbInit -->|"seeds"| SchoolCtx
    SchoolCtx -->|"maps"| Student
    SchoolCtx -->|"maps"| Course
    SchoolCtx -->|"maps"| Instructor
    SchoolCtx -->|"maps"| Department
    SchoolCtx -->|"maps"| Enrollment
    SchoolCtx -->|"maps"| OfficeAssignment
```
