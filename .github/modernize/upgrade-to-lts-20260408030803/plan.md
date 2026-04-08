# .NET Upgrade Plan: net6.0 → net10.0

## Overview

Upgrade **ContosoUniversity** from `.NET 6.0` to `.NET 10.0 (LTS)`.

- **Source framework**: net6.0
- **Target framework**: net10.0
- **Projects**: 1 (ContosoUniversity\ContosoUniversity.csproj)
- **Strategy**: All-At-Once

---

### Selected Strategy

**All-At-Once** — All projects upgraded simultaneously in a single operation.  
**Rationale**: 1 project on net6.0, straightforward upgrade — TFM bump + 4 package version bumps, no breaking API changes detected.

---

## Assessment Summary

| Category | Count |
|----------|-------|
| Projects | 1 |
| Mandatory issues | 1 (TFM change) |
| Potential issues | 4 (package upgrades) |
| Incompatible packages | 0 |

### Packages to Upgrade

| Package | Current Version | Target Version |
|---------|----------------|----------------|
| Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore | 6.0.2 | 10.0.5 |
| Microsoft.EntityFrameworkCore.SqlServer | 6.0.2 | 10.0.5 |
| Microsoft.EntityFrameworkCore.Tools | 6.0.2 | 10.0.5 |
| Microsoft.VisualStudio.Web.CodeGeneration.Design | 6.0.2 | 10.0.2 |

---

## Tasks

### Task 001 — Upgrade ContosoUniversity to net10.0

**Scope**: `ContosoUniversity\ContosoUniversity.csproj`

**Steps**:
1. Change `<TargetFramework>net6.0</TargetFramework>` → `<TargetFramework>net10.0</TargetFramework>` in `ContosoUniversity\ContosoUniversity.csproj`
2. Update package references in `ContosoUniversity\ContosoUniversity.csproj`:
   - `Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore` → `10.0.5`
   - `Microsoft.EntityFrameworkCore.SqlServer` → `10.0.5`
   - `Microsoft.EntityFrameworkCore.Tools` → `10.0.5`
   - `Microsoft.VisualStudio.Web.CodeGeneration.Design` → `10.0.2`
3. Run `dotnet restore` on the solution
4. Build solution (`dotnet build`) and fix all compilation errors in a single pass
5. Verify solution builds with 0 errors and 0 warnings (treat warnings as errors if applicable)
6. Run all tests (`dotnet test`) to confirm no regressions

**Success criteria**: Solution builds successfully, all existing tests pass.

---

## References

- Assessment: [assessment.md](assessment.md)
- Full package analysis: see assessment.md
