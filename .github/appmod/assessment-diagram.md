# ContosoUniversity - Application Architecture Diagram

## Overview
ContosoUniversity is an ASP.NET Core 6.0 web application that demonstrates a university management system using Razor Pages and Entity Framework Core with SQL Server.

## Architecture Components

### Presentation Layer
```
┌─────────────────────────────────────────────────────────────┐
│                    ASP.NET Core Web Application              │
│                         (.NET 6.0)                           │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Razor Pages:                                                │
│  ├─ Students (CRUD operations)                               │
│  ├─ Courses (CRUD operations)                                │
│  ├─ Instructors (CRUD operations)                            │
│  ├─ Departments (CRUD operations)                            │
│  ├─ About (Statistics)                                       │
│  └─ Index (Home)                                             │
│                                                              │
│  Static Content:                                             │
│  ├─ CSS (Bootstrap, custom styles)                           │
│  ├─ JavaScript (jQuery, Bootstrap, validation)               │
│  └─ Images/Icons                                             │
│                                                              │
└─────────────────────────────────────────────────────────────┘
                           │
                           │ HTTP/HTTPS
                           ▼
```

### Business/Data Layer
```
┌─────────────────────────────────────────────────────────────┐
│              Entity Framework Core 6.0.2                     │
│                  (ORM Layer)                                 │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  DbContext: SchoolContext                                    │
│                                                              │
│  Entities:                                                   │
│  ├─ Student                                                  │
│  ├─ Course                                                   │
│  ├─ Enrollment (Many-to-Many: Student ↔ Course)             │
│  ├─ Instructor                                               │
│  ├─ Department                                               │
│  └─ OfficeAssignment (One-to-One: Instructor)                │
│                                                              │
│  Features:                                                   │
│  ├─ Database Migrations                                      │
│  ├─ Automatic Database Initialization (DbInitializer)        │
│  └─ Many-to-Many Relationships (Course ↔ Instructor)         │
│                                                              │
└─────────────────────────────────────────────────────────────┘
                           │
                           │ SQL Queries
                           ▼
```

### Data Storage Layer
```
┌─────────────────────────────────────────────────────────────┐
│             SQL Server (LocalDB)                             │
│                                                              │
│  Database: SchoolContext                                     │
│                                                              │
│  Tables:                                                     │
│  ├─ Student                                                  │
│  ├─ Course                                                   │
│  ├─ Enrollment                                               │
│  ├─ Instructor                                               │
│  ├─ Department                                               │
│  ├─ OfficeAssignment                                         │
│  └─ CourseInstructor (Many-to-Many junction table)           │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

## Data Flow

```
┌──────────┐     HTTP      ┌──────────────┐    EF Core    ┌──────────────┐
│  Browser │ ◄────────────► │ Razor Pages  │ ◄────────────► │ SchoolContext│
│          │               │              │               │              │
└──────────┘               └──────────────┘               └──────────────┘
                                  │                               │
                                  │                               │
                                  ▼                               ▼
                           ┌──────────────┐               ┌──────────────┐
                           │Static Files  │               │  SQL Server  │
                           │(wwwroot)     │               │  (LocalDB)   │
                           └──────────────┘               └──────────────┘
```

## Key Technologies

| Component | Technology | Version |
|-----------|-----------|---------|
| Runtime | .NET | 6.0 |
| Framework | ASP.NET Core | 6.0 |
| ORM | Entity Framework Core | 6.0.2 |
| Database | SQL Server | LocalDB |
| UI Framework | Bootstrap | 5.x |
| Client Libraries | jQuery, jQuery Validation | Latest |
| Development | Visual Studio Code Generation | 6.0.2 |

## Application Features

### Student Management
- Create, Read, Update, Delete students
- Paginated student list
- Student enrollment tracking

### Course Management
- Manage course catalog
- Assign instructors to courses
- Track course credits and departments

### Instructor Management
- CRUD operations for instructors
- Office assignment tracking
- Course assignments (many-to-many)

### Department Management
- Manage academic departments
- Department budget tracking
- Administrator assignment

### Enrollment System
- Student-Course enrollment relationships
- Grade tracking
- Enrollment statistics

## Configuration

### Connection Strings
- **SchoolContext**: SQL Server LocalDB connection
- Database auto-creation and migration on startup
- Trusted connection with MultipleActiveResultSets

### Application Settings
- Page Size: 3 (for pagination)
- Logging: Information level (Warning for ASP.NET Core)
- HTTPS redirection enabled
- Developer exception page in Development mode

## Deployment Considerations

### Current Architecture
- **Environment**: Development/On-Premises
- **Database**: SQL Server LocalDB (development only)
- **Static Content**: Served directly from application

### Recommended Azure Migration Path

```
┌─────────────────────────────────────────────────────────────┐
│                    Azure Target Architecture                 │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌────────────────────┐         ┌──────────────────┐        │
│  │  Azure App Service │         │  Azure Blob      │        │
│  │  (Web App)         │────────►│  Storage + CDN   │        │
│  │  - Linux/Windows   │         │  (Static Content)│        │
│  └────────────────────┘         └──────────────────┘        │
│           │                                                  │
│           │                                                  │
│           ▼                                                  │
│  ┌────────────────────┐                                     │
│  │  Azure SQL         │                                     │
│  │  Database          │                                     │
│  │  or                │                                     │
│  │  SQL Managed       │                                     │
│  │  Instance          │                                     │
│  └────────────────────┘                                     │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Migration Recommendations

1. **Web Application**
   - Deploy to Azure App Service (Windows or Linux)
   - Supports .NET 6.0 natively
   - Compatible with AKS, ACA, or App Service Containers

2. **Database**
   - Migrate to Azure SQL Database for PaaS benefits
   - Alternative: SQL Managed Instance for advanced features
   - Update connection string to use Azure SQL

3. **Static Content**
   - Move to Azure Blob Storage with CDN
   - Reduces app hosting costs
   - Improves performance globally
   - Simplifies content updates (no app redeployment)

## Assessment Summary

### Issues Identified: 2

#### 1. Static Content Optimization (Scale.0001)
- **Severity**: Optional
- **Effort**: 3 story points
- **Impact**: 33 static files detected (CSS, JS, fonts)
- **Recommendation**: Migrate to Azure Blob Storage + CDN
- **Benefits**: Reduced costs, better performance, easier maintenance

#### 2. Connection String Migration (Connection.0001)
- **Severity**: Potential
- **Effort**: 3 story points
- **Impact**: SQL Server LocalDB connection detected
- **Recommendation**: Migrate to Azure SQL Database or Managed Instance
- **Benefits**: Cloud-native, scalability, managed backups, high availability

### Total Effort: 6 story points

### Target Platforms (All Compatible)
- Azure App Service (Windows/Linux)
- Azure Kubernetes Service (AKS)
- Azure Container Apps (ACA)
- Azure App Service Containers

## Security Considerations

- HTTPS redirection enabled
- Trusted connection to database
- No hardcoded credentials detected
- Developer exception page only in Development mode
- Database migrations run automatically on startup

## Next Steps

1. **Immediate Actions**
   - Review and plan Azure SQL Database migration
   - Evaluate Azure Blob Storage + CDN for static content
   
2. **Pre-Migration**
   - Update connection strings for Azure SQL
   - Configure Azure Blob Storage
   - Update application configuration for cloud environment
   
3. **Migration**
   - Deploy application to Azure App Service
   - Migrate database to Azure SQL
   - Move static content to Blob Storage + CDN
   
4. **Post-Migration**
   - Configure application insights for monitoring
   - Set up automated backups
   - Configure auto-scaling policies
   - Implement CI/CD pipeline

---

**Assessment Date**: 2026-02-07  
**Tool**: .NET AppCAT CLI v1.0.601  
**Framework**: .NET 6.0  
**Assessment Type**: Azure Migration Readiness
