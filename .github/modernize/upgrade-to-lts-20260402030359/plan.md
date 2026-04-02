# Upgrade Plan: ContosoUniversity .NET 6 → .NET 10

## Overview

Upgrade the **ContosoUniversity** web application from **.NET 6.0** to **.NET 10.0 LTS**.

- **Source**: net6.0
- **Target**: net10.0 (LTS)
- **Project**: `ContosoUniversity\ContosoUniversity.csproj`
- **Project type**: ASP.NET Core Web Application (SDK-style)

## Assessment Summary

**6 issues found** (1 mandatory, 5 potential):

| Rule | Severity | Description |
|------|----------|-------------|
| Project.0002 | Mandatory | Change TargetFramework from net6.0 to net10.0 |
| NuGet.0002 | Potential | Upgrade 4 NuGet packages to .NET 10 compatible versions |
| Api.0003 | Potential | Behavioral change: `UseExceptionHandler("/Error")` in Program.cs line 17 |

**Package updates required:**

| Package | Current | Target |
|---------|---------|--------|
| Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore | 6.0.2 | 10.0.5 |
| Microsoft.EntityFrameworkCore.SqlServer | 6.0.2 | 10.0.5 |
| Microsoft.EntityFrameworkCore.Tools | 6.0.2 | 10.0.5 |
| Microsoft.VisualStudio.Web.CodeGeneration.Design | 6.0.2 | 10.0.2 |

No security vulnerabilities detected. No incompatible packages.

## Selected Strategy

**All-At-Once** — All projects upgraded simultaneously in a single operation.  
**Rationale**: 1 project on .NET 6.0 (SDK-style), straightforward TFM bump with package version updates. No breaking API changes requiring structural rewrites.

---

## Tasks

### Task 01 — Upgrade ContosoUniversity to net10.0

**Scope**: `ContosoUniversity\ContosoUniversity.csproj`

1. In `ContosoUniversity\ContosoUniversity.csproj`, change `<TargetFramework>net6.0</TargetFramework>` to `<TargetFramework>net10.0</TargetFramework>`
2. Update NuGet package references in `ContosoUniversity\ContosoUniversity.csproj`:
   - `Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore`: `6.0.2` → `10.0.5`
   - `Microsoft.EntityFrameworkCore.SqlServer`: `6.0.2` → `10.0.5`
   - `Microsoft.EntityFrameworkCore.Tools`: `6.0.2` → `10.0.5`
   - `Microsoft.VisualStudio.Web.CodeGeneration.Design`: `6.0.2` → `10.0.2`
3. Run `dotnet restore` to resolve all updated dependencies
4. Build solution and fix all compilation errors:
   ```
   dotnet build ContosoUniversity.sln
   ```
5. Review `Program.cs` line 17 — `app.UseExceptionHandler("/Error")` has a behavioral change in .NET 10. Verify the exception handler route exists and behavior is as expected.
6. Run tests (if any): `dotnet test ContosoUniversity.sln`
7. Verify: solution builds with 0 errors and 0 warnings related to the upgrade

**Success criteria**: Solution builds successfully targeting net10.0 with all packages updated.
