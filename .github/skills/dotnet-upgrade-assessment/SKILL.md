---
name: dotnet-upgrade-assessment
description: Run .NET upgrade assessment using the AppModDotNetUpgrade MCP server to evaluate upgrade scenarios
---

# .NET Upgrade Assessment

This skill runs upgrade assessment for a .NET project using the AppModDotNetUpgrade MCP tools. It evaluates available upgrade scenarios and generates an assessment report without performing any actual upgrade.

## Input Parameters

- `workspace-path` (required): Path to the .NET project root (must contain a `.sln` or `.slnx` file)
- `assessment_dir` (required): Path to the assessment output directory
- `target-framework` (optional): Target .NET framework version for upgrade (e.g., `net8`, `net9`, `net10`). When provided, select the scenario matching this target. When omitted, use the most relevant scenario.

## Important Constraints

- **Assessment only** — do NOT perform any upgrade, code modification, or plan execution
- **Do NOT modify any source files** — this is a read-only analysis
- **Do NOT call `start_task` or `complete_task`** — no task execution
- `initialize_scenario` **must be called** before `generate_dotnet_upgrade_assessment` — the MCP tool only writes `assessment.md` to disk when an active scenario context exists. Without it, analysis runs but results are not persisted.
- Each assessment session for a different solution requires a **new** `initialize_scenario` call — never call `resume_scenario` when switching solutions, as that would reuse the previous solution's scenario folder and overwrite its artifacts.
- If any MCP tool call fails or returns an error, log the error and exit gracefully — do NOT retry indefinitely

## Execution Steps

### Step 1: Check Current State

Call `get_state()` to check the current workflow state and verify the MCP server is responsive.

### Step 2: Discover Available Scenarios

Call `get_scenarios()` to list available upgrade scenarios for this .NET project.

- If no scenarios are available, exit gracefully.
- If `target-framework` is provided, identify the scenario whose target matches the specified framework version.
- If `target-framework` is not provided, select the most relevant scenario (typically the latest supported target).

### Step 3: Load Scenario Details

For each discovered scenario (or the most relevant one if multiple exist):

Call `get_instructions(kind='scenario', query='<scenario_id>')` to load the scenario assessment details.

### Step 4: Initialize Scenario

Call `initialize_scenario(scenarioId='<scenario_id>', description='Assess <solution_name>')` to create a fresh scenario context.

This is **required** — `generate_dotnet_upgrade_assessment` only writes `assessment.md` to disk when an active scenario exists. The tool performs the analysis regardless, but without a scenario the results are never persisted.

### Step 5: Run the Assessment

Call `generate_dotnet_upgrade_assessment` with the solution path and target framework.

- If `target-framework` is provided, pass it as the target framework parameter.
- If `target-framework` is not provided, use the target framework from the selected scenario.

The MCP server writes its assessment output to the configured `outputPath` (set via `ua-settings.json`). Do NOT manually create or template report files — the MCP server handles output generation.

## Error Handling

- **MCP tools not available**: Exit gracefully — do NOT block the overall assessment.
- **No solution file found**: Exit gracefully (the caller already checks for this).
- **MCP tool call failed**: Log the error and exit. Do NOT retry indefinitely or let the failure propagate.
- **Timeout**: Exit gracefully with whatever progress was made.

## Success Criteria

- MCP tools called successfully (get_state, get_scenarios, get_instructions)
- No source files modified
- No upgrade scenarios initialized or executed
- Graceful handling of all error conditions
