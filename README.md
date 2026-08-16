# ZEISS SCM Analytics Co-Pilot

Agent specification and persistent memory schema for a technical co-pilot supporting the Global Supply Chain BI & Data Analytics function at ZEISS Medical Technologies, during the SAP APO → S/4HANA migration.

## Contents
- `SYSTEM_PROMPT.md` — the agent's role, domain scope, operating principles, and mandatory critique protocol.
- `MEMORY_SCHEMA.md` — documentation of the Supabase tables used to persist real-world context across sessions.

## Companion Infrastructure
- **Supabase project**: `zeiss-scm-copilot` — stores `context_log`, `decisions`, `migration_risks`, `kpi_snapshots`.

## Explicit Limitation
This agent does not have a live data connection into ZEISS's SAP/BW/Fabric systems. It reasons from what's logged in the memory store plus whatever is shared in-session. Building a real telemetry feed (API export job, scheduled sync, etc.) is a separate task.

## Next Steps
1. Populate `migration_risks` with the actual open risks from the current APO → S/4HANA program.
2. Wire at least one of the ~25 Python apps to write KPI snapshots into `kpi_snapshots` on each run.
3. Review and adjust the critique protocol in `SYSTEM_PROMPT.md` to match internal governance/review norms.
