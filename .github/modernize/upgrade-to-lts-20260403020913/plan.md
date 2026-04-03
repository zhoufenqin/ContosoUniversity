# .NET Upgrade Plan: ContosoUniversity

## Overview

Upgrade **ContosoUniversity** from **.NET 6.0** to **.NET 10.0** (latest LTS).

- **Source framework**: `net6.0`
- **Target framework**: `net10.0`
- **Project file**: `ContosoUniversity/ContosoUniversity.csproj`
- **Project type**: ASP.NET Core Razor Pages Web Application (SDK-style, `Microsoft.NET.Sdk.Web`)

The project already uses the modern SDK-style format and the minimal hosting model (`Program.cs` with top-level statements), so no structural conversion is required. The upgrade focuses on target framework and NuGet package version updates, plus resolving any breaking changes across .NET 7→8→9→10.

---

## Current State

### Target Framework
```xml
<TargetFramework>net6.0</TargetFramework>
```

### NuGet Packages (current)

| Package | Current Version |
|---------|----------------|
| `Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore` | 6.0.2 |
| `Microsoft.EntityFrameworkCore.SqlServer` | 6.0.2 |
| `Microsoft.EntityFrameworkCore.Tools` | 6.0.2 |
| `Microsoft.VisualStudio.Web.CodeGeneration.Design` | 6.0.2 |

---

## Target State

### Target Framework
```xml
<TargetFramework>net10.0</TargetFramework>
```

### NuGet Packages (target)

| Package | Target Version |
|---------|---------------|
| `Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore` | 10.0.0 |
| `Microsoft.EntityFrameworkCore.SqlServer` | 10.0.0 |
| `Microsoft.EntityFrameworkCore.Tools` | 10.0.0 |
| `Microsoft.VisualStudio.Web.CodeGeneration.Design` | 10.0.0 |

---

## Upgrade Tasks

### Task 1: Update Target Framework and NuGet Packages

**File**: `ContosoUniversity/ContosoUniversity.csproj`

1. Change `<TargetFramework>net6.0</TargetFramework>` to `<TargetFramework>net10.0</TargetFramework>`.
2. Update all four `<PackageReference>` entries from version `6.0.2` to `10.0.0`:
   - `Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore` → `10.0.0`
   - `Microsoft.EntityFrameworkCore.SqlServer` → `10.0.0`
   - `Microsoft.EntityFrameworkCore.Tools` → `10.0.0`
   - `Microsoft.VisualStudio.Web.CodeGeneration.Design` → `10.0.0`
3. Run `dotnet restore` to restore updated packages.
4. Run `dotnet build` to verify compilation.

**Expected result**: Project compiles successfully with `net10.0`.

---

### Task 2: Address Breaking Changes and Verify Runtime Behavior

After the build passes, address known breaking changes introduced between .NET 6 and .NET 10:

#### 2a. Program.cs — Developer Exception Page (ASP.NET Core 8+)

In .NET 8+, `UseDeveloperExceptionPage()` and `UseMigrationsEndPoint()` are no longer needed as explicit calls for apps using `WebApplication`—the behavior is auto-configured. However, these calls are still valid and will compile. **No change required** unless compiler warnings appear.

#### 2b. EF Core Breaking Changes (7 → 8 → 9 → 10)

- **EF Core 7**: Cascade delete behavior for owned entities changed. Review owned entity configurations in `Data/SchoolContext.cs`.
- **EF Core 8**: `DateOnly`/`TimeOnly` are now natively supported. No migration action needed unless custom value converters were in place.
- **EF Core 9+**: `HasConversion` and value-generator APIs may have changed. Review `Data/SchoolContext.cs` for any custom configurations.
- **EF Core 10**: Check for any obsolete API usages flagged by the compiler and update accordingly.

**File to review**: `ContosoUniversity/Data/SchoolContext.cs`

#### 2c. Existing EF Core Migrations

The existing migrations under `ContosoUniversity/Migrations/` were generated with EF Core 6.0. After upgrading to EF Core 10.0:

1. Run `dotnet ef database update` to verify existing migrations apply correctly.
2. If migration scaffolding changed, regenerate the initial migration snapshot:
   - `dotnet ef migrations add InitialCheck --output-dir Migrations` (only if an inconsistency is detected)

#### 2d. Nullable Reference Types

`<ImplicitUsings>enable</ImplicitUsings>` is already set. With .NET 10, null-safety enforcement may surface new warnings. Add `<Nullable>enable</Nullable>` to the project file if desired, and resolve any nullable warnings in:
- `ContosoUniversity/Models/` — model classes
- `ContosoUniversity/Data/SchoolContext.cs`
- `ContosoUniversity/Data/DbInitializer.cs`
- `ContosoUniversity/Pages/` — Razor Page models

#### 2e. Run Tests / Smoke Test

Run `dotnet run` and confirm:
- Application starts on HTTPS
- Home page loads
- Database migrates automatically (via `context.Database.Migrate()` in `Program.cs`)
- Razor Pages render correctly

---

## File Change Summary

| File | Change |
|------|--------|
| `ContosoUniversity/ContosoUniversity.csproj` | Update `TargetFramework` to `net10.0`; update all 4 packages to `10.0.0` |
| `ContosoUniversity/Data/SchoolContext.cs` | Review for EF Core breaking changes; update if needed |
| `ContosoUniversity/Migrations/` | Verify migrations apply; regenerate if needed |
| `ContosoUniversity/Program.cs` | Review for deprecated API usages; likely no changes needed |

---

## Notes

- The project is already using the SDK-style project format, minimal hosting model, and implicit usings — no structural changes needed.
- All four NuGet packages follow the same version as the .NET/EF Core major version, so upgrading all to `10.0.0` is correct.
- Use `dotnet nuget locals all --clear` if package restore cache issues arise.
