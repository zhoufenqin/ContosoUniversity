# Architecture Diagram

ContosoUniversity is an ASP.NET Core 6.0 Razor Pages web application that manages university students, courses, departments, and instructors using Entity Framework Core with SQL Server.

## Application Architecture

```mermaid
flowchart TD
    Browser["Browser\nHTTP Client"]

    subgraph App["ASP.NET Core 6.0 Web Application"]
        RazorPages["Razor Pages\nStudents / Courses / Departments / Instructors / About"]
        EFCore["Entity Framework Core 6.0\nSchoolContext DbContext"]
        Models["Domain Models\nStudent, Course, Enrollment\nInstructor, Department, OfficeAssignment"]
        DbInit["DbInitializer\nSeed Data"]
    end

    SqlServer["SQL Server\nLocalDB / mssqllocaldb\nSchoolContext Database"]

    Browser -- "HTTP requests" --> RazorPages
    RazorPages -- "uses" --> Models
    RazorPages -- "queries / commands" --> EFCore
    EFCore -- "uses" --> Models
    EFCore -- "SQL via EF Core SqlServer provider" --> SqlServer
    DbInit -- "seeds on startup" --> EFCore
```
