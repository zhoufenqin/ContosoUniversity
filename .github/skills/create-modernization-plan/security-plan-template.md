## Security Compliance

**Purpose**: Scan and remediate CVEs (Common Vulnerabilities and Exposures) in project dependencies to ensure the modernized application is free of known security vulnerabilities.

**Condition**: Always include this section in every modernization plan. If the user specifies additional security requirements, incorporate them into the description and requirements below.

**Template**:

**Description**: Scan all project dependencies for known CVEs and remediate any identified vulnerabilities to ensure the application is secure before deployment.

**Requirements**:
  Upgrade vulnerable dependencies to the minmum patched version within the same major version. If a CVE fix requires a major version upgrade, do NOT upgrade automatically. Instead, document the affected dependency, the current version, the required major version, and the breaking change risk. Verify that the project builds and all tests pass after remediation.
  If the user provided specific security requirements, incorporate them as well.

**Environment Configuration**:
  Runtime environment established by previous tasks (e.g., Java Home, .NET runtime).
  Build tool established by previous tasks (e.g., Maven/Gradle, dotnet).

**App Scope**:
  The app folders that this task will operate

**Skills**:
  - Skill Name: validate-cves-and-fix
    - Skill Location: builtin
  - Skill Name: [additional skill if needed]
    - Skill Location: [Skill location]