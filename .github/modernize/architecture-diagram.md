# Architecture Diagram

Contoso University is an ASP.NET Core 6.0 Razor Pages web application for managing university students, courses, instructors, and departments, backed by SQL Server via Entity Framework Core.

## System Context

This diagram shows how the Contoso University application interacts with external systems.

```mermaid
C4Context
    title System Context - Contoso University

    Person(user, "University Staff", "Manages students, courses, instructors, and departments")

    System(app, "Contoso University", "ASP.NET Core 6.0 Razor Pages web application")

    SystemDb(db, "SQL Server Database", "Stores students, courses, instructors, departments, and enrollments")

    Rel(user, app, "Uses", "HTTPS")
    Rel(app, db, "Reads and writes", "Entity Framework Core")
```

## Container View

This diagram shows the major containers within the application and how they communicate.

```mermaid
C4Container
    title Container View - Contoso University

    Person(user, "University Staff")

    Container_Boundary(app, "Contoso University Web App") {
        Container(web, "Razor Pages UI", "ASP.NET Core 6.0", "Handles HTTP requests and renders HTML pages for Students, Courses, Instructors, Departments")
        Container(dal, "Data Access Layer", "Entity Framework Core 6.0", "SchoolContext - manages database operations and migrations")
    }

    ContainerDb(db, "SQL Server", "Relational Database", "Stores School data: Students, Courses, Instructors, Departments, Enrollments")

    Rel(user, web, "Browses", "HTTPS")
    Rel(web, dal, "Uses")
    Rel(dal, db, "Reads and writes", "SQL over TCP")
```

## Component View

This diagram shows the internal components of the Contoso University web application.

```mermaid
C4Component
    title Component View - Contoso University Web App

    Container_Boundary(web, "Razor Pages UI") {
        Component(students, "Students Pages", "Razor Pages", "Index, Create, Edit, Delete, Details")
        Component(courses, "Courses Pages", "Razor Pages", "Index, Create, Edit, Delete, Details")
        Component(instructors, "Instructors Pages", "Razor Pages", "Index, Create, Edit, Delete, Details")
        Component(departments, "Departments Pages", "Razor Pages", "Index, Create, Edit, Delete, Details")
        Component(about, "About Page", "Razor Pages", "Enrollment statistics")
    }

    Container_Boundary(dal, "Data Access Layer") {
        Component(context, "SchoolContext", "DbContext", "EF Core database context with Students, Courses, Instructors, Departments, Enrollments")
        Component(initializer, "DbInitializer", "Seed Data", "Seeds initial data on startup")
    }

    ContainerDb(db, "SQL Server", "Relational Database")

    Rel(students, context, "Uses")
    Rel(courses, context, "Uses")
    Rel(instructors, context, "Uses")
    Rel(departments, context, "Uses")
    Rel(about, context, "Uses")
    Rel(context, db, "Reads and writes", "SQL")
    Rel(initializer, context, "Seeds via")
```
