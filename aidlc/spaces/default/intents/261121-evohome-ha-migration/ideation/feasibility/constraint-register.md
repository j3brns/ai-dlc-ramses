# Constraint Register — Ramses Heating Zone Programme Migration

## Sources

- [desc] Initial description: "Migrate Honeywell Evohome Heating zone programme controllers to home  assistant"
- [scope] Workflow-selected scope: `express`.
- [Q1] Integration targets.
- [Q2] No regulatory requirements.
- [Q3] Tech stack.
- [Q4] Timeline.
- [Q5] ramses_cc version.

## Technical Constraints

| ID | Constraint | Type | Impact | Source |
|---|---|---|---|---|
| TC-01 | Test-world only: no live RF, no live MQTT, no live control in Stage 1 | Hard | All HA automations and MQTT traffic must use isolated `RAMSES/TEST/...` topic namespace | [Q1, desc] |
| TC-02 | RAMSES-II RF protocol — not standard HA built-ins | Hard | Must use ramses_cc custom component; no standard HA thermostat integration applies | [Q1] |
| TC-03 | ramses_cc tracking upstream | Soft | API surface and MQTT topic structure may drift; pin version on proven build | [Q5] |
| TC-04 | CM67z schedule read-back not available in Stage 1 | Hard | Schedules must be manually transcribed into HA Schedule helpers for Stage 1 | [Q1] |
| TC-05 | HA version must support Schedule helper entity (2023.6+) | Soft | Verify HA version before build; document minimum version | [Q3] |
| TC-06 | Python 3.11+ required | Hard | Test infrastructure and ramses_rf library require Python 3.11+ | [Q3] |
| TC-07 | Zone count must not be hard-coded at 4 | Design | Schedule helper layer must support up to ~8 zones without structural changes | [Q1] |

## Organisational Constraints

| ID | Constraint | Type | Impact | Source |
|---|---|---|---|---|
| OC-01 | Household acceptance required before live cutover | Hard | Live control (Stage 2) cannot proceed without other residents accepting the system | [Q4, desc] |
| OC-02 | Single developer | Soft | No parallel workstreams; sequential delivery is assumed | [Q4] |

## Regulatory Constraints

None. Private home automation project with no applicable regulatory requirements. [Q2]

## Assumptions & Open Questions

- [assumption] No external budget or timeline constraint. [Q4]
- [assumption] No GDPR, HIPAA, or other regulatory obligation applies. [Q2]
