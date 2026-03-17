# Architecture Diagram

ContosoUniversity is an ASP.NET Core 6.0 Razor Pages web application using Entity Framework Core for data access against a SQL Server database.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        Browser["Web Browser"]
    end
    subgraph App["Application Layer - ASP.NET Core 6.0 Razor Pages"]
        Pages["Razor Pages\nStudents / Courses / Departments / Instructors"]
        Middleware["Middleware Pipeline\nHTTPS Redirect, Static Files, Routing, Authorization"]
        ViewModels["View Models\nAssignedCourseData, InstructorIndexData, EnrollmentDateGroup"]
        PaginatedList["PaginatedList Utility"]
    end
    subgraph Data["Data Layer"]
        EFCore["Entity Framework Core 6.0"]
        SchoolCtx[("SchoolContext\nDbContext")]
        DB[("SQL Server\nLocalDB")]
        DbInit["DbInitializer\nSeed Data"]
    end

    Browser -->|"HTTPS requests"| Middleware
    Middleware -->|"routes"| Pages
    Pages -->|"uses"| ViewModels
    Pages -->|"paginates with"| PaginatedList
    Pages -->|"queries / commands"| SchoolCtx
    SchoolCtx -->|"ORM mapping"| EFCore
    EFCore -->|"SQL queries"| DB
    DbInit -->|"seeds on startup"| SchoolCtx
```

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation - Razor Pages"]
        StudentsPages["Students Pages\nIndex, Create, Edit, Delete, Details"]
        CoursesPages["Courses Pages\nIndex, Create, Edit, Delete, Details"]
        DepartmentsPages["Departments Pages\nIndex, Create, Edit, Delete, Details"]
        InstructorsPages["Instructors Pages\nIndex, Create, Edit, Delete, Details"]
        HomePages["Home Pages\nIndex, About, Privacy, Error"]
    end
    subgraph Domain["Domain Models"]
        Student["Student"]
        Course["Course"]
        Department["Department"]
        Instructor["Instructor"]
        Enrollment["Enrollment"]
        OfficeAssignment["OfficeAssignment"]
    end
    subgraph ViewModelLayer["View Models"]
        AssignedCourseData["AssignedCourseData"]
        InstructorIndexData["InstructorIndexData"]
        EnrollmentDateGroup["EnrollmentDateGroup"]
        PaginatedList["PaginatedList"]
    end
    subgraph DataAccess["Data Access"]
        SchoolContext["SchoolContext"]
        DbInitializer["DbInitializer"]
    end

    StudentsPages -->|"reads/writes"| SchoolContext
    CoursesPages -->|"reads/writes"| SchoolContext
    DepartmentsPages -->|"reads/writes"| SchoolContext
    InstructorsPages -->|"reads/writes"| SchoolContext
    HomePages -->|"reads"| SchoolContext
    StudentsPages -->|"uses"| PaginatedList
    StudentsPages -->|"uses"| EnrollmentDateGroup
    InstructorsPages -->|"uses"| InstructorIndexData
    InstructorsPages -->|"uses"| AssignedCourseData
    CoursesPages -->|"uses"| AssignedCourseData
    SchoolContext -->|"manages"| Student
    SchoolContext -->|"manages"| Course
    SchoolContext -->|"manages"| Department
    SchoolContext -->|"manages"| Instructor
    SchoolContext -->|"manages"| Enrollment
    SchoolContext -->|"manages"| OfficeAssignment
    DbInitializer -->|"seeds via"| SchoolContext
```
