# Project Facts

## Application Overview

| Property | Value |
|----------|-------|
| Application Name | ContosoUniversity |
| Application Type | Web Application |
| Architecture Pattern | Razor Pages (ASP.NET Core) |
| Target Framework | .NET 6.0 |
| Primary Language | C# |
| Build Tool | MSBuild |

## Assessment Summary

| Metric | Value |
|--------|-------|
| Total Projects Analyzed | 1 |
| Total Issues Found | 2 |
| Total Incidents | 2 |
| Total Migration Effort | 6 story points |
| Mandatory Issues | 0 |
| Optional Issues | 1 |
| Potential Issues | 1 |
| Informational Issues | 0 |

## Dependencies

| Package | Version | Category |
|---------|---------|----------|
| Microsoft.NET.Sdk.Web (SDK) | 6.0 | Web Framework |
| Microsoft.EntityFrameworkCore.SqlServer | 6.0.2 | Database / ORM |
| Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore | 6.0.2 | Diagnostics |
| Microsoft.EntityFrameworkCore.Tools | 6.0.2 | Developer Tools |
| Microsoft.VisualStudio.Web.CodeGeneration.Design | 6.0.2 | Developer Tools |

## Identified Issues

### Scale.0001 — Optional (3 story points)
- **Location:** `ContosoUniversity/ContosoUniversity.csproj`
- **Severity:** Optional
- **Affected Targets:** Azure App Service, AKS, Azure Container Apps, Azure App Service Container, Azure App Service Managed Instance
- **Category:** Scale

### Connection.0001 — Potential (3 story points)
- **Location:** `ContosoUniversity/appsettings.json`
- **Severity:** Potential
- **Affected Targets:** Azure App Service, AKS, Azure Container Apps, Azure App Service Container, Azure App Service Managed Instance
- **Category:** Connection (hardcoded SQL Server LocalDB connection string)

## External Services

| Service | Type | Notes |
|---------|------|-------|
| SQL Server LocalDB | Database | Local development; needs Azure SQL or similar for cloud deployment |

## Assessment Tool

| Property | Value |
|----------|-------|
| Tool | .NET AppCAT CLI |
| Report Version | 1.0.0 |
| Privacy Mode | Restricted |
