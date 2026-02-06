# Application Assessment Instructions

## Overview
This document provides instructions for assessing the ContosoUniversity application using AppCAT (Application Containerization and Migration Analysis Tool).

## Project Information
- **Application**: ContosoUniversity
- **Platform**: .NET 6.0 (ASP.NET Core Web Application)
- **Framework**: Entity Framework Core with SQL Server
- **Project Type**: Web Application

## Platform Requirements

⚠️ **IMPORTANT**: .NET application assessment requires a **Windows operating system**.

The AppCAT assessment tool for .NET applications is only supported on Windows platforms. This limitation is due to the underlying .NET SDK and AppCAT CLI dependencies.

## Prerequisites

### Windows System Requirements
- Windows 10 or Windows 11
- Windows Server 2019 or later
- .NET SDK 6.0 or later installed

### Required Tools
1. **.NET SDK**: Download from https://dotnet.microsoft.com/download
2. **AppCAT CLI**: Will be automatically installed by the assessment tool

## Running the Assessment on Windows

### Option 1: Using GitHub Codespaces (Windows Container)
1. Create a Windows-based Codespace for this repository
2. Open the Codespace terminal
3. Navigate to the repository root
4. Run the assessment command (tool will install AppCAT CLI automatically)

### Option 2: Local Windows Machine
1. Clone the repository to your Windows machine:
   ```cmd
   git clone https://github.com/zhoufenqin/ContosoUniversity.git
   cd ContosoUniversity
   ```

2. Use the Assessment MCP server tools:
   - The tools will automatically detect this is a .NET project
   - AppCAT CLI will be installed if not already present
   - Assessment will analyze the code for cloud migration issues

3. The assessment report will be generated in:
   ```
   .github/appmod/dotnet-appcat/result/report.json
   ```

### Option 3: Using Windows GitHub Actions Runner
Create a GitHub Actions workflow that runs on Windows:

```yaml
name: .NET Application Assessment

on:
  workflow_dispatch:

jobs:
  assess:
    runs-on: windows-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup .NET
        uses: actions/setup-dotnet@v4
        with:
          dotnet-version: '6.0.x'
      
      - name: Run Assessment
        run: |
          # Assessment tools will be invoked here
          # Report will be generated in .github/appmod/
      
      - name: Upload Assessment Report
        uses: actions/upload-artifact@v4
        with:
          name: assessment-report
          path: .github/appmod/
```

## Expected Output

After successful assessment, you should find:

1. **Main Report**: `.github/appmod/report.json`
   - Consolidated assessment report
   - Should be committed to the repository

2. **Detailed Results**: `.github/appmod/dotnet-appcat/result/`
   - `report.json`: Full AppCAT analysis
   - Additional analysis files and logs

## Report Contents

The assessment report includes:

- **Application Profile**
  - Detected frameworks (ASP.NET Core, Entity Framework)
  - Dependencies and NuGet packages
  - Target framework version

- **Migration Issues**
  - Mandatory changes required for cloud migration
  - Potential issues to review
  - Optional improvements

- **Recommendations**
  - Azure service recommendations
  - Containerization guidance
  - Modernization opportunities

## Current Status

❌ **Assessment Not Completed**
- Reason: Current environment is Ubuntu Linux
- Required: Windows operating system
- Action Needed: Run assessment on Windows platform using one of the options above

## Next Steps

1. Choose one of the Windows-based options above
2. Run the assessment on a Windows system
3. Commit the generated `.github/appmod/report.json` to the repository
4. Review the findings and recommendations

## Support

For issues or questions about:
- **AppCAT Tool**: Check Microsoft AppCAT documentation
- **Repository Issues**: Create an issue in this repository
- **.NET Migration**: Consult Azure App Service migration guides
