# Architecture Diagram

Application architecture showing logical layers and data flow.

## Application Architecture

The following diagram shows the logical layers of the application and how they interact.

```mermaid
graph TD
    A[Presentation Layer\nPages - Students, Courses, Departments, Instructors] --> B[Business Logic Layer\nPage Models, View Models, Domain Models]
    B --> C[Data Access Layer\nDatabase Context, Repository, Initializer]
    C --> D[Data Storage\nRelational Database]
```
