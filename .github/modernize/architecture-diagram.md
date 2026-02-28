# Architecture Diagram

Application architecture showing logical layers and data flow.

## Application Architecture

This diagram illustrates the layered structure of the application and how data flows between layers.

```mermaid
graph TD
    A[Presentation Layer\nPages and Views] --> B[Business Logic Layer\nPage Models]
    B --> C[Data Access Layer\nContext and Initializer]
    C --> D[Domain Models\nStudents, Courses, Departments, Instructors]
    A --> E[Static Content\nClient-side Assets]
```
