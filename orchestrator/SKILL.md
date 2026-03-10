# Base Workflow Orchestrator Skill

## Description

**This is the MASTER CONTROLLER for the Base Chain automation suite. It is the ONLY tool used to coordinate trading-core, risk-monitor, and yield-optimizer.** Use this skill whenever the user wants full automation, scheduled tasks (Cron), or a complete system status report.

### Trigger Keywords

- "automate", "full cycle", "daily routine", "workflow", "run everything", "coordinate skills"

---

## Allowed Tools

- **orchestrator**: Master coordinator for autonomous Base chain operations.
  - **Command**: `node ./scripts/main.cjs`
  - **Description**: Primary entry point for automation. Accepts natural language (e.g., "run a full cycle") or specific keywords (status, daily, full-cycle).
  - **Arguments**: Pass the user's intent or action keyword as a string.

### Action Matrix

| Action Key | Result |
|:---|:---|
| `status` | Provides a snapshot of the entire system and wallet health. |
| `daily` | Generates a performance and yield report for the last 24h. |
| `full-cycle` | Executes a complete Loop: Risk Check -> Yield Analysis -> Trade Execution. |

---

## Integration & Security

- **Interoperability**: Automatically triggers sub-skills (trading-core, etc.).
- **Safety**: Includes built-in DRY_RUN validation and pre-execution security audits.
