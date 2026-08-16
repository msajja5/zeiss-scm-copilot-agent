# Agent Memory Store — Supabase Schema

Project: `zeiss-scm-copilot` (ref: mdidtsrzwmrtrbmxcljm, region eu-west-1)

## Tables

### context_log
General running log of real-world situations, incidents, and observations relevant to the migration/analytics ecosystem.
| column | type | notes |
|---|---|---|
| id | bigint identity | PK |
| logged_at | timestamptz | default now() |
| domain | text | e.g. 'ETL', 'forecasting', 'dashboard', 'migration' |
| system_area | text | e.g. 'APO-DP', 'S/4HANA', 'Fabric', 'BW' |
| summary | text | short description |
| raw_detail | text | longer notes, logs, error text |
| severity | text | info / watch / risk / critical |
| created_by | text | who logged it |

### decisions
Decisions made during the journey, so the agent doesn't re-litigate settled questions.
| column | type | notes |
|---|---|---|
| id | bigint identity | PK |
| decided_at | timestamptz | default now() |
| topic | text | |
| decision | text | what was decided |
| rationale | text | why |
| alternatives_considered | text | |
| status | text | proposed / approved / rejected / superseded |
| owner | text | |

### migration_risks
Tracks specific SAP APO → S/4HANA migration risks affecting the analytics ecosystem.
| column | type | notes |
|---|---|---|
| id | bigint identity | PK |
| identified_at | timestamptz | default now() |
| component | text | e.g. 'Demand Planning extractor' |
| legacy_system | text | e.g. 'APO DP' |
| target_system | text | e.g. 'IBP Demand' |
| risk_description | text | |
| impact | text | low / medium / high / critical |
| mitigation | text | |
| status | text | open / mitigating / resolved / accepted |

### kpi_snapshots
Point-in-time KPI values so trends can be discussed across sessions.
| column | type | notes |
|---|---|---|
| id | bigint identity | PK |
| captured_at | timestamptz | default now() |
| kpi_name | text | e.g. 'OTIF', 'MAPE' |
| value | numeric | |
| unit | text | e.g. '%', 'days' |
| dimension | text | e.g. plant, region, product line |
| source_app | text | which of the ~25 Python apps produced it |
| notes | text | |

## Usage Pattern
At the start of a session, query recent rows (e.g. last 30 days) from all four tables to re-establish context. After any material recommendation, decision, or newly discovered risk, insert a row so the next session inherits it.
