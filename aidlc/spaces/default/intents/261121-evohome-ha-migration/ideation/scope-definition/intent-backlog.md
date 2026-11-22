# Intent Backlog — Ramses Heating Zone Programme Migration

## Sources

- [desc] Initial description: "Migrate Honeywell Evohome Heating zone programme controllers to home  assistant"
- [scope] Workflow-selected scope: `express`.
- Upstream: `aidlc/spaces/default/intents/261121-evohome-ha-migration/ideation/intent-capture/intent-statement.md`
- Upstream: `aidlc/spaces/default/intents/261121-evohome-ha-migration/ideation/feasibility/feasibility-assessment.md`
- Upstream: `aidlc/spaces/default/intents/261121-evohome-ha-migration/ideation/feasibility/constraint-register.md`
- [Q1] Minimum viable scope.
- [Q2] Priority classification.
- [Q3] Sequencing: risk-first.

## Prioritised Intent Backlog (MoSCoW)

| ID | Capability | Priority | Depends on | Rationale | Source |
|---|---|---|---|---|---|
| IB-01 | Define and document MQTT test isolation mechanism (topic-namespace or broker ACL) | Must | — | Unblocks all HA automation wiring; mis-routing causes live device commands (RI-02) | [Q2, Q3, feasibility] |
| IB-02 | Pin ramses_cc to a proven version; document upgrade test procedure | Must | — | Prevents silent API drift breaking schedule dispatch (RI-01) | [Q2, Q3, feasibility] |
| IB-03 | Confirm HA version ≥ 2023.6; document minimum version | Must | — | Prerequisite for Schedule helper entities | [Q2, feasibility] |
| IB-04 | Create HA Schedule helpers for all 4 zones (7-day, multi-period) | Must | IB-01, IB-02, IB-03 | Core schedule representation in HA | [Q1, Q2, intent-statement] |
| IB-05 | Build HA automations: schedule → zone setpoint dispatch via MQTT test transport | Must | IB-04 | Proves the schedule is actionable in HA | [Q1, Q2] |
| IB-06 | Write deterministic pytest tests: correct setpoint per zone per time slot | Must | IB-05 | Satisfies "evidence then move" principle; check.ps1 must pass | [Q1, Q2, feasibility] |
| IB-07 | Add cross-check fixture test: HA Schedule output vs. CM67z reference programme | Must | IB-04 | Catches manual transcription errors (RI-04) | [Q2, feasibility] |
| IB-08 | Lovelace zone card: temperature, target, mode, next scheduled change (all 4 zones) | Must | IB-04, IB-05 | Household visibility; supports acceptance | [Q2, intent-statement] |

## Delivery Sequence

```
Phase A — Infrastructure decisions (IB-01, IB-02, IB-03) — parallel, no dependencies
    |
Phase B — Schedule layer (IB-04)
    |
Phase C — Dispatch automations + test coverage (IB-05, IB-06, IB-07) — IB-05 → IB-06, IB-07 parallel after IB-04
    |
Phase D — Lovelace (IB-08) — after IB-04 and IB-05
```

## Won't Have (This Stage)

| Capability | Reason |
|---|---|
| Live control | Test-world only |
| CM67z schedule read-back (automated) | Future capability |
| Optimum start live learning | Needs real data |
| Presence / weather integration | Explicitly out of scope |
| Custom Lovelace frontend | Standard cards only |

## Assumptions & Open Questions

- [assumption] All 8 backlog items are Must-have for Stage 1; none are nice-to-have.
- [assumption] Zone count will stay at 4 for Stage 1 but the Schedule helper layer must support ~8.
