# Scope Document — Ramses Heating Zone Programme Migration

## Sources

- [desc] Initial description: "Migrate Honeywell Evohome Heating zone programme controllers to home  assistant"
- [scope] Workflow-selected scope: `express`.
- Upstream: `aidlc/spaces/default/intents/261121-evohome-ha-migration/ideation/intent-capture/intent-statement.md`
- Upstream: `aidlc/spaces/default/intents/261121-evohome-ha-migration/ideation/feasibility/feasibility-assessment.md`
- Upstream: `aidlc/spaces/default/intents/261121-evohome-ha-migration/ideation/feasibility/constraint-register.md`
- [Q1] Minimum viable scope confirmed.
- [Q2] Must-have vs. nice-to-have confirmed.
- [Q3] Sequencing: risk-first chosen by conductor per owner instruction.
- [Q4] No hard deadlines.

## Stage 1 Scope Boundary (This Work)

### In Scope

| Capability | Priority | Notes | Source |
|---|---|---|---|
| MQTT test isolation mechanism defined and documented | Must-have | Topic-namespace vs. broker ACL decision; unblocks all HA automation wiring | [Q2, Q3, feasibility RI-02] |
| ramses_cc version pinned on proven build | Must-have | Prevents silent API breakage on HA update; unblocks schedule dispatch | [Q2, Q3, feasibility RI-01] |
| HA version confirmed as 2023.6+ (Schedule helper support) | Must-have | Prerequisite for Schedule helper entities | [Q2, feasibility AS-03] |
| HA Schedule helpers for all 4 zones (Controller 487: Kitchen, EJMB+RDGB; Controller 751: Lounge, CMQ JGB) | Must-have | 7-day, multi-period programme per zone; manually transcribed from CM67z | [Q1, Q2, intent-statement] |
| HA automation: setpoint dispatch per zone on schedule | Must-have | Sends correct target temperature to each Climate entity at scheduled time, via MQTT test transport | [Q1, Q2] |
| Deterministic test coverage: schedule → setpoint dispatch path | Must-have | pytest tests; check.ps1 gate must pass; covers correct setpoint per zone per time slot | [Q1, Q2, feasibility RI-04] |
| Lovelace card per zone: temperature, target, mode, next change | Must-have | Standard HA cards; no custom frontend | [Q2, intent-statement] |
| Design supports up to ~8 zones (no hard-coded 4-zone ceiling) | Must-have | Zone config must be parameterised | [Q2, intent-statement, constraint TC-07] |

### Out of Scope (Stage 1)

| Capability | Reason | Future stage |
|---|---|---|
| Live RF transmission | Test-world only; no live control until Stage 2 | Stage 2 |
| Live MQTT (non-TEST topics) | Test-world only | Stage 2 |
| CM67z thermostat decommissioning | Retained as fallback | Stage 3 |
| Automated schedule read-back from CM67z via RAMSES-II | Not supported in Stage 1; manual transcription used | Future |
| Presence detection, weather, or other HA device integration | Explicitly out of scope | Future |
| Boiler interlock or boiler control hardware changes | Separate concern; not touched | Future |
| Custom Lovelace frontend components | Standard cards only | Future |
| optimum-start live learning | Needs real bounded evidence; not in test-world scope | Future |

## Sequencing

Risk-first ordering: [Q3]

1. **Infrastructure decisions** — MQTT isolation mechanism, ramses_cc pin, HA version check
2. **Schedule layer** — HA Schedule helpers (all 4 zones), setpoint dispatch automations
3. **Test coverage** — pytest tests for schedule → dispatch path, check.ps1 gate
4. **Lovelace visibility** — zone cards

## Success Definition

Stage 1 is complete when: [Q1, Q2]
- All four zone schedules are in HA Schedule helpers with correct 7-day programmes.
- Deterministic tests demonstrate correct setpoint dispatch via isolated MQTT test transport.
- Lovelace shows temperature, target, mode, and next change for each zone.
- `check.ps1` passes with no regressions.
- No live devices are affected.

## Assumptions & Open Questions

- [assumption] Stage 1 is test-world only; no live RF or live MQTT traffic.
- [assumption] The sequencing is risk-first: infrastructure decisions before schedule layer build.
