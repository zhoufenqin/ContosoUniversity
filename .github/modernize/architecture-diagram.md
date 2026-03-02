# Architecture Diagram

ContosoUniversity is an ASP.NET Core 6.0 Razor Pages web application that manages university students, courses, departments, and instructors using Entity Framework Core with SQL Server.

## Application Architecture

```mermaid
flowchart TD
    Browser["Browser\nHTTP Client"]

    subgraph WebApp["ASP.NET Core 6.0 Web Application"]
        RazorPages["Razor Pages\nStudents / Courses / Departments / Instructors"]
        EFCore["Entity Framework Core 6.0\nSchoolContext DbContext"]
        Models["Domain Models\nStudent, Course, Department, Instructor, Enrollment"]
    end

    SQLServer["SQL Server\nLocalDB / SQL Server\nSchoolContext Database"]

    Browser -- "HTTP Requests" --> RazorPages
    RazorPages -- "Uses" --> Models
    RazorPages -- "Queries and Updates" --> EFCore
    EFCore -- "Maps models to tables" --> Models
    EFCore -- "SQL Queries" --> SQLServer
```
