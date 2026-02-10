# Modernization Assessment Summary

**Target Azure Services**: Azure App Service, Azure Kubernetes Service, Azure Container Apps, Azure App Service Container

## Overall Statistics

**Total Applications**: 1

**Name: ContosoUniversity**
- Mandatory: 0 issues
- Potential: 1 issues
- Optional: 1 issues

> **Severity Levels Explained:**
> - **Mandatory**: The issue has to be resolved for the migration to be successful.
> - **Potential**: This issue may be blocking in some situations but not in others. These issues should be reviewed to determine whether a change is required or not.
> - **Optional**: The issue discovered is real issue fixing which could improve the app after migration, however it is not blocking.

## Applications Profile

### Name: ContosoUniversity
- **Frameworks**: net6.0
- **Languages**: C#
- **Build Tools**: MSBuild

**Key Findings**:
- **Potential Issues (1 locations)**:
  - <!--ruleid=Connection.0001-->Connection string is detected (1 location found)
- **Optional Issues (1 locations)**:
  - <!--ruleid=Scale.0001-->Static content detected (1 location found)

## Next Steps

For comprehensive migration guidance and best practices, visit:
- [GitHub Copilot modernization](https://aka.ms/ghcp-appmod)
