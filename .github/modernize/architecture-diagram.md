# Architecture Diagram

This is an ASP.NET Core 6.0 Razor Pages web application for managing university students, courses, instructors, and departments, backed by SQL Server via Entity Framework Core.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        Browser["Web Browser"]
    end
    subgraph App["Application Layer - ASP.NET Core 6.0 Razor Pages"]
        Pages["Razor Pages"]
        Middleware["Middleware Pipeline\n(HTTPS, Static Files, Routing, Authorization)"]
        DI["Dependency Injection Container"]
    end
    subgraph Data["Data Layer"]
        EF["Entity Framework Core 6.0"]
        DB[("SQL Server LocalDB")]
    end

    Browser -->|"HTTP/HTTPS requests"| Middleware
    Middleware -->|"routes to"| Pages
    Pages -->|"injects"| DI
    DI -->|"resolves"| EF
    EF -->|"SQL queries"| DB
    DB -->|"seeded by"| DbInit["DbInitializer"]
```

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        StudentsPages["Students Pages\n(Index/Create/Edit/Delete/Details)"]
        CoursesPages["Courses Pages\n(Index/Create/Edit/Delete/Details)"]
        InstructorsPages["Instructors Pages\n(Index/Create/Edit/Delete/Details)"]
        DepartmentsPages["Departments Pages\n(Index/Create/Edit/Delete/Details)"]
        HomePages["Home Pages\n(Index/About/Privacy/Error)"]
    end
    subgraph Business["Business Logic"]
        PaginatedList["PaginatedList"]
        Utility["Utility"]
        InstructorCoursesBase["InstructorCoursesPageModel\n(base)"]
        DeptNameBase["DepartmentNamePageModel\n(base)"]
    end
    subgraph DataAccess["Data Access"]
        SchoolContext["SchoolContext\n(DbContext)"]
        DbInitializer["DbInitializer"]
    end
    subgraph Entities["Entities"]
        Student["Student"]
        Course["Course"]
        Instructor["Instructor"]
        Department["Department"]
        Enrollment["Enrollment"]
        OfficeAssignment["OfficeAssignment"]
    end

    StudentsPages -->|"paginates"| PaginatedList
    StudentsPages -->|"queries"| SchoolContext
    CoursesPages -->|"uses"| DeptNameBase
    CoursesPages -->|"queries"| SchoolContext
    InstructorsPages -->|"uses"| InstructorCoursesBase
    InstructorsPages -->|"queries"| SchoolContext
    DepartmentsPages -->|"uses"| Utility
    DepartmentsPages -->|"queries"| SchoolContext
    HomePages -->|"queries"| SchoolContext
    SchoolContext -->|"maps"| Student
    SchoolContext -->|"maps"| Course
    SchoolContext -->|"maps"| Instructor
    SchoolContext -->|"maps"| Department
    SchoolContext -->|"maps"| Enrollment
    SchoolContext -->|"maps"| OfficeAssignment
    DbInitializer -->|"seeds"| SchoolContext
```
