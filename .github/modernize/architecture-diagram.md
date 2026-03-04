# Architecture Diagram

ContosoUniversity is an ASP.NET Core Razor Pages web application that manages university data including students, courses, instructors, and departments using Entity Framework Core with SQL Server.

## Application Architecture

```mermaid
flowchart TD
    User["User\n(Web Browser)"]

    subgraph App["ContosoUniversity - ASP.NET Core 6.0"]
        subgraph Presentation["Presentation Layer\n(Razor Pages)"]
            Students["Students Pages"]
            Courses["Courses Pages"]
            Instructors["Instructors Pages"]
            Departments["Departments Pages"]
        end

        subgraph BusinessLogic["Application Layer"]
            PaginatedList["PaginatedList\n(Pagination Utility)"]
            DbInit["DbInitializer\n(Seed Data)"]
        end

        subgraph DataAccess["Data Access Layer\n(Entity Framework Core 6.0)"]
            SchoolContext["SchoolContext\n(DbContext)"]
            Models["Domain Models\nStudent, Course, Enrollment\nInstructor, Department\nOfficeAssignment"]
        end
    end

    subgraph Storage["Data Storage"]
        SQLServer["SQL Server\n(localdb / mssqllocaldb)"]
    end

    subgraph Static["Static Content"]
        WWWRoot["wwwroot\n(CSS, JS, Images)"]
    end

    User -->|"HTTP/HTTPS requests"| Presentation
    Presentation --> BusinessLogic
    Presentation --> DataAccess
    BusinessLogic --> DataAccess
    DataAccess --> SchoolContext
    SchoolContext --> Models
    SchoolContext -->|"EF Core Migrations"| SQLServer
    App -->|"Serves static files"| Static
```
