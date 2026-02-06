# ContosoUniversity - Application Assessment Summary

## Assessment Overview

- **Assessment Date**: 2026-02-06
- **Tool**: .NET AppCAT CLI (version 1.0.601)
- **Privacy Mode**: Protected
- **Analysis Duration**: ~4 seconds
- **Target Platforms**: Azure App Service, Azure Kubernetes Service, Azure Container Apps

## Project Details

- **Application Name**: ContosoUniversity
- **Framework**: .NET 6.0
- **Language**: C#
- **Build Tool**: MSBuild
- **Project Type**: ASP.NET Core Web Application

## Assessment Results

### Summary
- **Total Projects Analyzed**: 1
- **Total Issues Found**: 2
- **Total Incidents**: 2
- **Total Effort (Story Points)**: 6

### Issue Breakdown by Severity
- **Mandatory**: 0
- **Optional**: 1
- **Potential**: 1
- **Information**: 0

### Issue Breakdown by Category
- **Scale**: 1 issue
- **Connection**: 1 issue

## Detailed Issues

### 1. Static Content Detected (Scale.0001)

**Severity**: Optional  
**Effort**: 3 story points  
**Location**: ContosoUniversity/ContosoUniversity.csproj

**Description**:  
Your current approach of serving static content directly might lead to increased costs, performance issues, maintenance challenges (requiring application redeployment for content changes), and security risks.

**Files Affected** (33 files):
- wwwroot/css/site.css
- wwwroot/favicon.ico
- wwwroot/js/site.js
- wwwroot/lib/bootstrap/dist/* (Bootstrap CSS/JS files)
- wwwroot/lib/jquery/* (jQuery files)
- wwwroot/lib/jquery-validation/* (Validation files)

**Recommendation**:  
Consider moving static content to Azure Blob Storage and adding Azure CDN for improved performance, reduced costs, and better scalability.

**Resources**:
- [Azure Blob Storage](https://go.microsoft.com/fwlink/?linkid=2250574)
- [Azure CDN](https://go.microsoft.com/fwlink/?linkid=2250392)

### 2. Connection String Detected (Connection.0001)

**Severity**: Potential  
**Effort**: 3 story points  
**Location**: ContosoUniversity/appsettings.json

**Description**:  
Connection string detected. It may not be available when your app is migrated to Azure. Review and ensure it works from Azure. If you are connecting to a database, you might need to migrate the database to Azure.

**Options for Database Migration**:
1. Migrate to Azure SQL Database for additional benefits
2. Consider Azure SQL Managed Instance if you need features that are not available in Azure SQL Database
3. Migrate your database servers directly to SQL Server on Azure VMs

**Recommendation**:  
Use Azure Migrate tool to assess database migration effort and choose the appropriate Azure SQL service based on your requirements.

**Resources**:
- [Migrate SQL Server database to Azure](https://go.microsoft.com/fwlink/?LinkID=2251731)
- [Azure SQL Managed Instance](https://go.microsoft.com/fwlink/?LinkID=2251613)
- [Azure Migrate](https://go.microsoft.com/fwlink/?linkid=2252410)

## Target Platforms

The application was analyzed for compatibility with the following Azure platforms:
- Azure App Service (Windows & Linux)
- Azure Kubernetes Service (Windows & Linux)
- Azure Container Apps
- Azure App Service Container (Windows & Linux)
- Azure App Service Managed Instance (Windows)

## Next Steps

1. **Review Connection Strings**: Update connection strings in appsettings.json to use Azure-compatible configurations (e.g., Azure SQL Database connection strings)
2. **Optimize Static Content**: Evaluate moving static assets (CSS, JS, images) to Azure Blob Storage with CDN
3. **Plan Database Migration**: Assess current database and plan migration to Azure SQL Database or Azure SQL Managed Instance
4. **Security Review**: Ensure connection strings use Azure Key Vault or managed identity for secure credential management

## Assessment Report

The full assessment report is available at: `.github/appmod/report.json`
