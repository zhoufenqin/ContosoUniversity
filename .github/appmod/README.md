# Application Assessment Directory

This directory contains the application assessment results for the ContosoUniversity .NET application.

## Directory Structure

```
.github/appmod/
├── README.md                      # This file
├── ASSESSMENT_INSTRUCTIONS.md     # Detailed instructions for running assessment
├── report.json                    # Consolidated assessment report (to be generated on Windows)
└── dotnet-appcat/                 # Detailed AppCAT results (generated during assessment)
    └── result/
        └── report.json            # Full AppCAT analysis report
```

## Status

⚠️ **Platform Requirement**: This assessment requires a Windows operating system.

The current system is Ubuntu Linux, but .NET application assessment via AppCAT is only supported on Windows.

## How to Complete the Assessment

Please see [ASSESSMENT_INSTRUCTIONS.md](./ASSESSMENT_INSTRUCTIONS.md) for detailed instructions on running the assessment on a Windows platform.

## Quick Start (Windows Only)

If you're on a Windows system with the Assessment MCP server tools available:

1. Run precheck:
   ```
   Use Assessment MCP tool: appmod-precheck-assessment
   with workspacePath: /path/to/ContosoUniversity
   ```

2. Run assessment:
   ```
   Use Assessment MCP tool: appmod-run-assessment
   with workspacePath: /path/to/ContosoUniversity
   ```

3. The report will be automatically generated in this directory.

## Report Information

Once generated, the `report.json` file will contain:

- Application profile (frameworks, dependencies, build tools)
- Migration issues categorized by severity
- Specific code locations requiring attention
- Recommendations for Azure migration
- Containerization readiness analysis

## Next Steps After Assessment

1. Review the generated `report.json`
2. Address any mandatory issues identified
3. Plan migration strategy based on recommendations
4. Consider containerization options if applicable
