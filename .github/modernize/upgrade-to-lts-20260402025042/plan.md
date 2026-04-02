# Upgrade Plan: ContosoUniversity to .NET 10.0

## Overview

**Source**: .NET 6.0 (`net6.0`)  
**Target**: .NET 10.0 LTS (`net10.0`)  
**Solution**: `ContosoUniversity.sln`  
**Project**: `ContosoUniversity/ContosoUniversity.csproj`

### Selected Strategy
**All-At-Once** — All projects upgraded simultaneously in a single operation.  
**Rationale**: 1 project on .NET 6.0 (SDK-style), clear dependency structure, straightforward TFM/package bumps with minor API fixes.

---

## Assessment Summary

| Category | Count |
|----------|-------|
| Projects | 1 |
| Total Issues | 10 (1 mandatory, 9 potential) |
| Affected Files | 2 |
| NuGet packages to upgrade | 4 |
| Incompatible packages | 0 |

**Files with issues:**
- `ContosoUniversity/ContosoUniversity.csproj` — TFM change + 4 package upgrades
- `ContosoUniversity/Program.cs` — 4 source-incompatible API usages + 1 behavioral change

---

## Tasks

### Task 01: Upgrade ContosoUniversity to .NET 10.0

**Scope**: `ContosoUniversity/ContosoUniversity.csproj` and `ContosoUniversity/Program.cs`

**Steps:**

#### 1. Update Target Framework

In `ContosoUniversity/ContosoUniversity.csproj`, change:
```xml
<TargetFramework>net6.0</TargetFramework>
```
to:
```xml
<TargetFramework>net10.0</TargetFramework>
```

#### 2. Update NuGet Package References

In `ContosoUniversity/ContosoUniversity.csproj`, update all four package references:

| Package | Current Version | Target Version |
|---------|----------------|----------------|
| `Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore` | 6.0.2 | 10.0.5 |
| `Microsoft.EntityFrameworkCore.SqlServer` | 6.0.2 | 10.0.5 |
| `Microsoft.EntityFrameworkCore.Tools` | 6.0.2 | 10.0.5 |
| `Microsoft.VisualStudio.Web.CodeGeneration.Design` | 6.0.2 | 10.0.2 |

#### 3. Fix Source-Incompatible API Usages in Program.cs

**Line 10** — `builder.Services.AddDatabaseDeveloperPageExceptionFilter()`:
- This API is flagged as source-incompatible (`Api.0002`) for net10.0.
- After upgrading `Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore` to 10.0.5, attempt to build; if compilation fails, remove the call (only needed in Development environments for EF migration error pages).
- If still needed, wrap in `if (app.Environment.IsDevelopment())` guard and verify the API remains in version 10.0.5.

**Line 24** — `app.UseMigrationsEndPoint()`:
- This API is flagged as source-incompatible (`Api.0002`) for net10.0.
- After upgrading `Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore` to 10.0.5, attempt to build; if compilation fails, remove or replace with `app.UseEndpoints()` / EF Core tooling alternative.
- If the API exists in 10.0.5, keep it; otherwise remove it — the Migrations endpoint is only useful in Development.

**Line 17** — `app.UseExceptionHandler("/Error")`:
- Behavioral change (`Api.0003`): In .NET 8+, `UseExceptionHandler(string)` throws `InvalidOperationException` at startup if no exception handler is registered for the provided path. Ensure the `/Error` page/route is properly registered, or switch to `app.UseExceptionHandler(options => { ... })` overload.

#### 4. Restore, Build, and Validate

```bash
dotnet restore ContosoUniversity.sln
dotnet build ContosoUniversity.sln
```

Fix any remaining compilation errors introduced by the framework or package upgrade before proceeding.

#### 5. Run Tests

```bash
dotnet test ContosoUniversity.sln
```

---

## Success Criteria

- [ ] `ContosoUniversity/ContosoUniversity.csproj` targets `net10.0`
- [ ] All 4 NuGet packages updated to .NET 10.0-compatible versions
- [ ] `Program.cs` compiles without errors against net10.0
- [ ] `dotnet build ContosoUniversity.sln` exits with code 0
- [ ] All existing tests pass
