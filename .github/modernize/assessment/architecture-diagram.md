# Architecture Diagram

Contoso University is an ASP.NET Core 6.0 Razor Pages web application that manages university data (students, courses, instructors, and departments) using Entity Framework Core with a SQL Server backend.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        Browser["Web Browser"]
    end
    subgraph App["Application Layer - ASP.NET Core 6.0 Razor Pages"]
        Pages["Razor Pages\n(Students / Courses / Departments / Instructors)"]
        Middleware["Middleware Pipeline\n(Routing, Static Files, Authorization, HTTPS)"]
        DbInit["DbInitializer\n(Seed Data on Startup)"]
    end
    subgraph DataLayer["Data Layer"]
        Context["SchoolContext\n(Entity Framework Core 6)"]
        DB[("SQL Server\n(LocalDB)")]
    end

    Browser -->|"HTTP/HTTPS requests"| Middleware
    Middleware -->|"routes"| Pages
    Pages -->|"reads and writes"| Context
    DbInit -->|"seeds on startup"| Context
    Context -->|"SQL queries"| DB
```

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        StudPages["Students Pages\n(Index / Create / Edit / Details / Delete)"]
        CoursePages["Courses Pages\n(Index / Create / Edit / Details / Delete)"]
        DeptPages["Departments Pages\n(Index / Create / Edit / Details / Delete)"]
        InstrPages["Instructors Pages\n(Index / Create / Edit / Details / Delete)"]
        HomePages["Home Pages\n(Index / About / Privacy / Error)"]
    end
    subgraph BaseModels["Base Page Models"]
        DeptBase["DepartmentNamePageModel"]
        InstrBase["InstructorCoursesPageModel"]
    end
    subgraph ViewModels["View Models"]
        AssignedCourse["AssignedCourseData"]
        EnrollDateGroup["EnrollmentDateGroup"]
        InstrIndexData["InstructorIndexData"]
    end
    subgraph DomainModels["Domain Models"]
        Student["Student"]
        Course["Course"]
        Enrollment["Enrollment"]
        Instructor["Instructor"]
        Department["Department"]
        OfficeAssign["OfficeAssignment"]
    end
    subgraph DataAccess["Data Access"]
        SchoolCtx["SchoolContext\n(DbContext)"]
        DbInit["DbInitializer"]
    end

    CoursePages -->|"extends"| DeptBase
    InstrPages -->|"extends"| InstrBase
    InstrBase -->|"uses"| AssignedCourse
    HomePages -->|"uses"| EnrollDateGroup
    InstrPages -->|"uses"| InstrIndexData
    StudPages -->|"queries"| SchoolCtx
    CoursePages -->|"queries"| SchoolCtx
    DeptPages -->|"queries"| SchoolCtx
    InstrPages -->|"queries"| SchoolCtx
    HomePages -->|"queries"| SchoolCtx
    SchoolCtx -->|"manages"| Student
    SchoolCtx -->|"manages"| Course
    SchoolCtx -->|"manages"| Enrollment
    SchoolCtx -->|"manages"| Instructor
    SchoolCtx -->|"manages"| Department
    SchoolCtx -->|"manages"| OfficeAssign
    DbInit -->|"seeds"| SchoolCtx
```
