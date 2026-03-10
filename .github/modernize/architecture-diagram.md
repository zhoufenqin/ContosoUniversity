# Architecture Diagram

ContosoUniversity is an ASP.NET Core Razor Pages web application for managing a university's student enrollment system, built on .NET 6 with Entity Framework Core and SQL Server.

## Application Architecture

```mermaid
flowchart TB
    subgraph Client["Client Browser"]
        B["Bootstrap 5\nCSS + JS"]
        JQ["jQuery 3.x\nValidation"]
    end

    subgraph Presentation["Presentation Layer - ASP.NET Core Razor Pages"]
        HOME["Home / About Pages"]
        STU["Students Pages\nCRUD + Pagination + Search"]
        CRS["Courses Pages\nCRUD"]
        INS["Instructors Pages\nCRUD + Course Assignment"]
        DEP["Departments Pages\nCRUD + Concurrency"]
    end

    subgraph BusinessLogic["Business Logic - Page Models"]
        PM["Page Models\nASP.NET Core 6.0"]
        PL["PaginatedList\nPagination Helper"]
        LINQ["LINQ Queries\nAsync/Await"]
    end

    subgraph DataAccess["Data Access Layer - Entity Framework Core 6.0"]
        CTX["SchoolContext\nDbContext"]
        INIT["DbInitializer\nSeed Data"]
        MIG["EF Core Migrations"]
    end

    subgraph Database["Data Storage - SQL Server"]
        SQLDB["SQL Server\nLocalDB"]
        subgraph Schema["Database Schema"]
            T1["Students"]
            T2["Enrollments"]
            T3["Courses"]
            T4["Instructors"]
            T5["Departments"]
            T6["OfficeAssignments"]
        end
    end

    subgraph Static["Static Content - wwwroot"]
        CSS["CSS\nsite.css"]
        JS["JS\nsite.js"]
        LIB["Libraries\nBootstrap, jQuery"]
    end

    Client -->|"HTTP Requests"| Presentation
    Presentation -->|"Page Model calls"| BusinessLogic
    BusinessLogic -->|"DbContext queries"| DataAccess
    DataAccess -->|"SQL queries"| Database
    Static -->|"Served to browser"| Client

    T1 -->|"1 to many"| T2
    T3 -->|"1 to many"| T2
    T5 -->|"1 to many"| T3
    T4 -->|"many to many"| T3
    T4 -->|"1 to 1"| T6
```
