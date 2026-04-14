# .NET Upgrade Plan: ContosoUniversity

## Overview

Upgrade **ContosoUniversity** from **.NET 6.0** to **.NET 10.0 (LTS)**.

- **Source**: net6.0
- **Target**: net10.0 (LTS)
- **Projects**: 1 (`ContosoUniversity\ContosoUniversity.csproj`)
- **Assessment**: 10 issues found (1 mandatory, 9 potential)

---

### Selected Strategy

**All-At-Once** — All projects upgraded simultaneously in a single operation.  
**Rationale**: 1 project on .NET 6.0, straightforward upgrade with TFM bump, package version bumps, and targeted code fixes.

---

## Tasks

### Task 01 — Upgrade ContosoUniversity to net10.0

**ID**: `01-upgrade-contosouniversity`

**Scope**: `ContosoUniversity\ContosoUniversity.csproj`

**What to do**:

1. **Update target framework** in `ContosoUniversity\ContosoUniversity.csproj`:
   - Change `<TargetFramework>net6.0</TargetFramework>` → `<TargetFramework>net10.0</TargetFramework>`

2. **Update NuGet package references** in `ContosoUniversity\ContosoUniversity.csproj`:
   - `Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore` 6.0.2 → **10.0.5**
   - `Microsoft.EntityFrameworkCore.SqlServer` 6.0.2 → **10.0.5**
   - `Microsoft.EntityFrameworkCore.Tools` 6.0.2 → **10.0.5**
   - `Microsoft.VisualStudio.Web.CodeGeneration.Design` 6.0.2 → **10.0.2**

3. **Fix source incompatibilities** in `ContosoUniversity\Program.cs`:
   - Line 10: `builder.Services.AddDatabaseDeveloperPageExceptionFilter()` — `DatabaseDeveloperPageExceptionFilterServiceExtensions` API is source-incompatible with .NET 10. Remove or replace as needed (this method is removed in .NET 10; the middleware is no longer required).
   - Line 24: `app.UseMigrationsEndPoint()` — `MigrationsEndPointExtensions.UseMigrationsEndPoint` API is source-incompatible with .NET 10. Remove or replace with the updated equivalent (requires `Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore` 10.x).

4. **Address behavioral change** in `ContosoUniversity\Program.cs`:
   - Line 17: `app.UseExceptionHandler("/Error")` — behavioral change in .NET 10 where `UseExceptionHandler(string)` overload now requires explicit error handler registration. Verify the behavior is correct after upgrade; update if needed.

5. **Restore dependencies**: `dotnet restore`

6. **Build and fix compilation errors**: `dotnet build`
   - Fix any remaining compilation errors in a single pass.

7. **Run tests** (if test projects exist): `dotnet test`

**Success Criteria**:
- Solution builds with 0 errors
- All existing tests pass
