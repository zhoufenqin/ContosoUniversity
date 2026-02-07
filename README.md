ContosoUniversity
=================

A sample ASP.NET Core 6.0 web application demonstrating Entity Framework Core and Razor Pages for building data-driven web applications.

## What is ContosoUniversity?

ContosoUniversity is an educational sample application that simulates a university management system. It showcases:

- **ASP.NET Core 6.0** - Modern web framework
- **Entity Framework Core** - Object-relational mapping (ORM)
- **Razor Pages** - Page-based UI programming model
- **SQL Server** - Relational database
- **CRUD Operations** - Create, Read, Update, Delete functionality

## Features

- 📚 **Student Management** - Track students and their enrollment information
- 📖 **Course Management** - Manage course catalog with credits and departments
- 👨‍🏫 **Instructor Management** - Faculty information and course assignments
- 🏢 **Department Management** - Academic departments with budgets
- 🔗 **Complex Relationships** - Demonstrates one-to-many and many-to-many relationships
- 📄 **Pagination** - Efficient handling of large datasets
- ✅ **Data Validation** - Built-in validation using data annotations

## Domain Model

The application includes the following entities:
- **Student** - Student information and enrollments
- **Course** - Course details, credits, and department
- **Enrollment** - Links students to courses with grades
- **Instructor** - Faculty members and their courses
- **Department** - Academic departments
- **OfficeAssignment** - Instructor office locations

## Getting Started

### Prerequisites
- [.NET 6.0 SDK](https://dotnet.microsoft.com/download/dotnet/6.0)
- SQL Server or SQL Server LocalDB

### Running the Application

1. Clone the repository
2. Navigate to the ContosoUniversity folder
3. Update the connection string in `appsettings.json` if needed
4. Run the application:
   ```bash
   dotnet run
   ```
5. Open your browser to `https://localhost:5001` (or the URL displayed in the console)

The application will automatically:
- Create the database
- Run migrations
- Seed sample data

## Project Structure

```
ContosoUniversity/
├── Models/              # Domain entities
├── Data/               # DbContext and database initialization
├── Pages/              # Razor Pages (UI)
├── Migrations/         # EF Core migrations
├── wwwroot/           # Static files
└── Program.cs         # Application startup
```

## Learning Resources

This project is based on Microsoft's official tutorial series:
- [Get started with Razor Pages in ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/)
- [Contoso University with Razor Pages](https://docs.microsoft.com/aspnet/core/data/ef-rp/)

## Documentation

For a detailed explanation of the project architecture, features, and learning objectives, see [PROJECT_EXPLANATION.md](PROJECT_EXPLANATION.md).

## License

This is a sample application for educational purposes.
