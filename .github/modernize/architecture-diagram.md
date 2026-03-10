# Architecture Diagram

This diagram represents the architecture of the Contoso University ASP.NET Core web application, showing its layers, data flow, and key dependencies.

## Application Architecture

```mermaid
flowchart TD
    Browser["Browser\n(HTML / CSS / JS)"]

    subgraph Presentation["Presentation Layer (Razor Pages)"]
        Students["Students Pages\n(Index, Create, Edit, Delete, Details)"]
        Courses["Courses Pages\n(Index, Create, Edit, Delete, Details)"]
        Departments["Departments Pages\n(Index, Create, Edit, Delete, Details)"]
        Instructors["Instructors Pages\n(Index, Create, Edit, Delete, Details)"]
        About["About / Index / Privacy Pages"]
        Static["Static Assets\n(Bootstrap, jQuery, wwwroot)"]
    end

    subgraph BusinessLogic["Business Logic Layer"]
        PageModels["Razor Page Models\n(ASP.NET Core 6.0)"]
        PaginatedList["PaginatedList Utility"]
        ViewModels["View Models\n(InstructorIndexData, EnrollmentDateGroup,\nAssignedCourseData)"]
    end

    subgraph DataLayer["Data Access Layer"]
        EFCore["Entity Framework Core 6.0\n(ORM)"]
        SchoolContext["SchoolContext\n(DbContext)"]
        Migrations["EF Migrations"]
        DbInitializer["DbInitializer\n(Seed Data)"]
    end

    subgraph Models["Domain Models"]
        Student["Student"]
        Course["Course"]
        Enrollment["Enrollment"]
        Instructor["Instructor"]
        Department["Department"]
        OfficeAssignment["OfficeAssignment"]
    end

    subgraph DataStorage["Data Storage"]
        SQLServer["SQL Server\n(LocalDB / mssqllocaldb)"]
    end

    Browser -->|HTTP requests| Presentation
    Static -->|served by| Browser
    Presentation --> PageModels
    PageModels --> PaginatedList
    PageModels --> ViewModels
    PageModels --> SchoolContext
    SchoolContext --> EFCore
    EFCore --> Migrations
    EFCore --> DbInitializer
    SchoolContext --> Models
    EFCore -->|SQL queries| SQLServer
```
