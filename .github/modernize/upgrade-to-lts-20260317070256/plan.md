# .NET Upgrade Plan: ContosoUniversity (net6.0 → net10.0)

## Executive Summary

This plan upgrades the ContosoUniversity ASP.NET Core Razor Pages application from **.NET 6.0** to **.NET 10.0 (LTS)**.

### Selected Strategy
**All-At-Once Strategy** — All changes applied simultaneously in a single coordinated operation.

**Rationale**:
- 1 project (small solution — well below the 5-project threshold)
- Currently on net6.0 with a clean, SDK-style project file
- 4,263 lines of code (low complexity)
- All 4 NuGet packages have direct .NET 10.0-compatible versions available
- 2 files with API compatibility issues, all resolvable during compilation

---

## Source and Target Versions

| Property | Value |
|---|---|
| **Source Framework** | net6.0 |
| **Target Framework** | net10.0 |
| **Project** | `ContosoUniversity\ContosoUniversity.csproj` |
| **Project Type** | ASP.NET Core Razor Pages (SDK-style) |

---

## Implementation Timeline

### Phase 1: Atomic Upgrade

**Operations** (performed as a single coordinated batch):
1. Update `TargetFramework` in `ContosoUniversity\ContosoUniversity.csproj` from `net6.0` to `net10.0`
2. Update all 4 NuGet package references in `ContosoUniversity\ContosoUniversity.csproj`
3. Restore dependencies (`dotnet restore`)
4. Build the solution to surface compilation errors (`dotnet build`)
5. Fix all compilation errors from the API breaking changes listed in the Breaking Changes Catalog
6. Rebuild to verify 0 errors and 0 warnings

**Deliverables**: Solution builds with 0 errors and 0 warnings

### Phase 2: Validation

**Operations**:
- Run full solution build
- Verify application starts and pages respond correctly
- Run any existing tests

**Deliverables**: Application verified functional on .NET 10.0

---

## Detailed Execution Steps

### Step 1: Update Project Target Framework

File: `ContosoUniversity\ContosoUniversity.csproj`

Change:
```xml
<TargetFramework>net6.0</TargetFramework>
```
To:
```xml
<TargetFramework>net10.0</TargetFramework>
```

### Step 2: Update NuGet Package References

Update all package references in `ContosoUniversity\ContosoUniversity.csproj`:

| Package | Current Version | Target Version |
|---|---|---|
| `Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore` | 6.0.2 | 10.0.5 |
| `Microsoft.EntityFrameworkCore.SqlServer` | 6.0.2 | 10.0.5 |
| `Microsoft.EntityFrameworkCore.Tools` | 6.0.2 | 10.0.5 |
| `Microsoft.VisualStudio.Web.CodeGeneration.Design` | 6.0.2 | 10.0.2 |

After update, `ContosoUniversity.csproj` `<ItemGroup>` should be:
```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore" Version="10.0.5" />
  <PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer" Version="10.0.5" />
  <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="10.0.5">
    <PrivateAssets>all</PrivateAssets>
    <IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
  </PackageReference>
  <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="10.0.2" />
</ItemGroup>
```

### Step 3: Restore and Build

```bash
dotnet restore ContosoUniversity.sln
dotnet build ContosoUniversity.sln
```

### Step 4: Fix Breaking Changes

See §Breaking Changes Catalog below. All API issues are in `ContosoUniversity\Program.cs`.

### Step 5: Rebuild and Verify

```bash
dotnet build ContosoUniversity.sln
```

Expected outcome: 0 errors, 0 warnings.

---

## Package Update Reference

### All Package Updates (ContosoUniversity.csproj)

| Package | Current | Target | Update Reason |
|---|---|---|---|
| `Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore` | 6.0.2 | 10.0.5 | Framework compatibility upgrade |
| `Microsoft.EntityFrameworkCore.SqlServer` | 6.0.2 | 10.0.5 | Framework compatibility upgrade |
| `Microsoft.EntityFrameworkCore.Tools` | 6.0.2 | 10.0.5 | Framework compatibility upgrade |
| `Microsoft.VisualStudio.Web.CodeGeneration.Design` | 6.0.2 | 10.0.2 | Framework compatibility upgrade |

---

## Breaking Changes Catalog

All breaking changes are isolated to `ContosoUniversity\Program.cs`.

### 1. `AddDatabaseDeveloperPageExceptionFilter` — Source Incompatible

**Location**: `ContosoUniversity\Program.cs`, line 11  
**Current code**:
```csharp
builder.Services.AddDatabaseDeveloperPageExceptionFilter();
```
**Issue**: `DatabaseDeveloperPageExceptionFilterServiceExtensions.AddDatabaseDeveloperPageExceptionFilter` is source-incompatible with the updated `Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore` 10.0.5 package. The API may have a changed signature or namespace.  
**Resolution**: After upgrading the package, recompile and resolve any resulting compilation error. The method is still available but may require updated parameters or using directive. Refer to the compiler error message for the exact fix.

### 2. `UseMigrationsEndPoint` — Source Incompatible

**Location**: `ContosoUniversity\Program.cs`, line 25  
**Current code**:
```csharp
app.UseMigrationsEndPoint();
```
**Issue**: `MigrationsEndPointExtensions.UseMigrationsEndPoint(IApplicationBuilder)` is source-incompatible with the updated package. The extension may have moved to a different type or require a different parameter.  
**Resolution**: After upgrading the package, recompile and resolve any compilation error. Update method call or using directive as directed by the compiler.

### 3. `UseExceptionHandler("/Error")` — Behavioral Change

**Location**: `ContosoUniversity\Program.cs`, line 18  
**Current code**:
```csharp
app.UseExceptionHandler("/Error");
```
**Issue**: `ExceptionHandlerExtensions.UseExceptionHandler(IApplicationBuilder, string)` has a behavioral change in .NET 10. The exception handling behavior or status code handling may differ.  
**Resolution**: Test exception handling end-to-end after the upgrade to verify error pages render correctly. No code change may be required, but runtime validation is needed.

---

## Project-by-Project Upgrade Plan

### Project: ContosoUniversity\ContosoUniversity.csproj

| Property | Value |
|---|---|
| **Current Framework** | net6.0 |
| **Target Framework** | net10.0 |
| **Project Kind** | ASP.NET Core Razor Pages |
| **Lines of Code** | 4,263 |
| **Files** | 78 (2 with API incidents) |
| **Risk** | 🟢 Low |
| **Dependencies** | None |
| **Dependants** | None |

**Upgrade Steps**:
1. Update `<TargetFramework>` to `net10.0`
2. Update all 4 `<PackageReference>` versions (see Package Update Reference)
3. Fix `AddDatabaseDeveloperPageExceptionFilter` compilation issue in `Program.cs`
4. Fix `UseMigrationsEndPoint` compilation issue in `Program.cs`
5. Validate `UseExceptionHandler` behavioral change at runtime

**Validation**:
- [ ] `dotnet build` succeeds with 0 errors
- [ ] `dotnet build` succeeds with 0 warnings
- [ ] Application starts successfully
- [ ] Student, Course, Department, Instructor pages load correctly
- [ ] Error page renders when exception is triggered

---

## Testing Strategy

### Build Validation
After completing all upgrade steps:
```bash
dotnet build ContosoUniversity.sln --no-incremental
```
Expected: 0 errors, 0 warnings.

### Functional Testing
Since there are no automated test projects in this solution, manual functional testing is required:

- [ ] Home page loads
- [ ] Students list, create, edit, delete operations work
- [ ] Courses list, create, edit, delete operations work
- [ ] Departments list, create, edit, delete operations work
- [ ] Instructors list, create, edit, delete operations work
- [ ] About page with enrollment statistics loads
- [ ] 404/error page renders correctly (validates `UseExceptionHandler` behavioral change)
- [ ] Database migration runs on startup without errors

---

## Risk Management

### Risk Assessment

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| `AddDatabaseDeveloperPageExceptionFilter` API change requires code update | Medium | Low | Resolve after first `dotnet build`; compiler will indicate exact fix |
| `UseMigrationsEndPoint` API change requires code update | Medium | Low | Resolve after first `dotnet build`; compiler will indicate exact fix |
| `UseExceptionHandler` behavioral change breaks error pages | Low | Low | Manually test error page in development and production environments |
| EF Core migration incompatibility with .NET 10 | Low | Medium | Run `dotnet ef database update` and verify migrations apply cleanly |

### Rollback Plan
The upgrade is done on branch `upgrade-to-NET10`. If the upgrade cannot be completed, abandon the branch and return to `app-modernize-20260317015832`.

---

## Source Control

All upgrade changes should be committed as a **single commit** on branch `upgrade-to-NET10`:

```
chore: upgrade ContosoUniversity from net6.0 to net10.0

- Update TargetFramework to net10.0
- Update Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore 6.0.2 → 10.0.5
- Update Microsoft.EntityFrameworkCore.SqlServer 6.0.2 → 10.0.5
- Update Microsoft.EntityFrameworkCore.Tools 6.0.2 → 10.0.5
- Update Microsoft.VisualStudio.Web.CodeGeneration.Design 6.0.2 → 10.0.2
- Fix API breaking changes in Program.cs
```

---

## Success Criteria

The upgrade is complete when **all** of the following are true:

- [ ] `ContosoUniversity\ContosoUniversity.csproj` targets `net10.0`
- [ ] All 4 NuGet packages updated to target versions
- [ ] `dotnet build ContosoUniversity.sln` succeeds with 0 errors and 0 warnings
- [ ] Application starts and database migrates successfully
- [ ] All CRUD pages for Students, Courses, Departments, and Instructors function correctly
- [ ] Error handling (`UseExceptionHandler`) verified at runtime
- [ ] Changes committed to `upgrade-to-NET10` branch
