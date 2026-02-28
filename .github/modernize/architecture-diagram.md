# Architecture Diagram

Application architecture showing logical layers and data flow.

## Application Architecture

This diagram illustrates the layered structure of the application and how data flows between layers.

```mermaid
graph TD
    A[Presentation Layer\nPages / Views] --> B[Business Logic Layer\nPage Models]
    B --> C[Data Access Layer\nDbContext]
    C --> D[Data Store\nDatabase]
    A --> E[Static Content\nwwwroot]
```
