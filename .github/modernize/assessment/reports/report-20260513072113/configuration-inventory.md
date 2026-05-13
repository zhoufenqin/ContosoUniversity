# Configuration & Externalized Settings Inventory

ContosoUniversity has a compact configuration surface centered on ASP.NET Core appsettings files, launch profiles, and local service dependency metadata. The application uses environment-based overrides for development behavior, stores one database connection string in configuration, and does not introduce external config servers or feature-flag services.

## Configuration Sources

| Source | Type | Path/Location | Notes |
| --- | --- | --- | --- |
| `appsettings.json` | ASP.NET Core application configuration | `ContosoUniversity/appsettings.json` | Defines page size, logging defaults, allowed hosts, and the `SchoolContext` connection string |
| `appsettings.Development.json` | Environment-specific override | `ContosoUniversity/appsettings.Development.json` | Enables detailed errors and development logging behavior |
| `launchSettings.json` | Local launch profile configuration | `ContosoUniversity/Properties/launchSettings.json` | Defines HTTP/HTTPS local URLs and sets `ASPNETCORE_ENVIRONMENT=Development` for local runs |
| `serviceDependencies.json` | Service dependency metadata | `ContosoUniversity/Properties/serviceDependencies.json` | Declares an `mssql` dependency bound to `ConnectionStrings:SchoolContext` |
| `serviceDependencies.local.json` | Local dependency metadata | `ContosoUniversity/Properties/serviceDependencies.local.json` | Declares a local MSSQL dependency for developer environments |
| `ContosoUniversity.csproj` | Build and framework configuration | `ContosoUniversity/ContosoUniversity.csproj` | Selects `net6.0`, the web SDK, and NuGet package versions |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
| --- | --- | --- | --- |
| Debug | Standard `dotnet build` / `dotnet run` default for local development | Builds the application with developer-oriented settings | Uses the same project dependencies; no custom MSBuild profile additions were found |
| Release | Explicit `-c Release` build configuration | Produces optimized build output for deployment | Uses the same project dependencies; no additional publish-time packages were found |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
| --- | --- | --- | --- |
| Default / non-development | Application starts without `ASPNETCORE_ENVIRONMENT=Development` | `appsettings.json` | Uses standard logging, allowed hosts, and production-style exception handling/HSTS |
| Development | `ASPNETCORE_ENVIRONMENT=Development` from launch profiles | `appsettings.json`, `appsettings.Development.json`, `launchSettings.json` | Enables detailed errors, developer exception page, EF migrations endpoint, and local HTTP/HTTPS URLs |
| IIS Express | Visual Studio or local IIS Express profile | `launchSettings.json` | Uses IIS Express URLs while still setting the environment to Development |

## Properties Inventory

| Property Key | Default | Profiles | Source |
| --- | --- | --- | --- |
| `PageSize` | `3` | Default, Development | `appsettings.json` |
| `Logging:LogLevel:Default` | `Information` | Default, Development | `appsettings.json` |
| `Logging:LogLevel:Microsoft.AspNetCore` | `Warning` | Default, Development | `appsettings.json` |
| `AllowedHosts` | `*` | Default, Development | `appsettings.json` |
| `ConnectionStrings:SchoolContext` | `[MASKED local SQL Server connection string]` | Default, Development | `appsettings.json`, referenced by service dependency metadata |
| `DetailedErrors` | `true` | Development only | `appsettings.Development.json` |
| `profiles:ContosoUniversity:applicationUrl` | `https://localhost:7192;http://localhost:5202` | Development local run | `launchSettings.json` |
| `profiles:IIS Express:applicationUrl` | `http://localhost:31248` with SSL port `44319` | Development local run | `launchSettings.json` |
| `profiles:*:environmentVariables:ASPNETCORE_ENVIRONMENT` | `Development` | Development local run, IIS Express | `launchSettings.json` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
| --- | --- | --- | --- |
| ContosoUniversity | No explicit runtime flags, container args, or custom process parameters discovered | Not specified in repository | Single instance implied by current project structure |

## Startup Dependency Chain

1. `Program` builds the web host and loads configuration from appsettings and environment variables.
2. Dependency injection creates `SchoolContext` using the `SchoolContext` connection string.
3. On startup, the application opens a scoped context, runs EF Core migrations, and executes `DbInitializer.Initialize`.
4. After database readiness is confirmed, middleware and Razor Pages routes begin serving requests.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
| --- | --- | --- |
| `ConnectionStrings:SchoolContext` | SQL Server connection string | `appsettings.json` `[MASKED]` |

### Secrets Provisioning Workflow

The repository stores the database connection string directly in `appsettings.json` for local development and maps service dependency metadata back to the same configuration key. No Key Vault, secret manager, managed identity, or CI/CD secret-provisioning workflow was found in the checked-in files, so secret distribution appears to be configuration-file-based in the current setup.

## Feature Flags

| Flag Name | Default | Controlled By |
| --- | --- | --- |
| None discovered | n/a | No feature-flag framework or conditional feature toggle configuration found |

## Framework & Runtime Versions

| Component | Version | Source |
| --- | --- | --- |
| .NET target framework | `net6.0` | `ContosoUniversity.csproj` |
| ASP.NET Core web SDK | `Microsoft.NET.Sdk.Web` for .NET 6 | `ContosoUniversity.csproj` |
| Entity Framework Core SQL Server provider | `6.0.2` | `ContosoUniversity.csproj` |
| Entity Framework Core tools | `6.0.2` | `ContosoUniversity.csproj` |
| ASP.NET Core EF diagnostics package | `6.0.2` | `ContosoUniversity.csproj` |
| Web code generation tooling | `6.0.2` | `ContosoUniversity.csproj` |
