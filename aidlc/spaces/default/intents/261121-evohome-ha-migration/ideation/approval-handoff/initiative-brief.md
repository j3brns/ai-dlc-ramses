# Initiative Brief — Ramses Heating Zone Programme Migration

## Sources

- [desc] Initial description: "Migrate Honeywell Evohome Heating zone programme controllers to home  assistant"
- [scope] Workflow-selected scope: `express`.
- Upstream: `aidlc/spaces/default/intents/261121-evohome-ha-migration/ideation/intent-capture/intent-statement.md`
- Upstream: `aidlc/spaces/default/intents/261121-evohome-ha-migration/ideation/intent-capture/stakeholder-map.md`
- Upstream: `aidlc/spaces/default/intents/261121-evohome-ha-migration/ideation/scope-definition/scope-document.md`
- Upstream: `aidlc/spaces/default/intents/261121-evohome-ha-migration/ideation/scope-definition/intent-backlog.md`
- Upstream: `aidlc/spaces/default/intents/261121-evohome-ha-migration/ideation/feasibility/feasibility-assessment.md`
- Upstream: `aidlc/spaces/default/intents/261121-evohome-ha-migration/ideation/feasibility/constraint-register.md`
- [Q1] Stakeholder alignment confirmed.
- [Q2] All critical risks acknowledged with mitigations.
- [Q3] Solo developer, no budget constraint.

## Intent & Problem Statement

The house has four independently programmed heating zones across two Honeywell CM67z programmable room thermostats (Controllers 487 and 751). Each zone's weekly heating programme runs in isolation with no unified view or control. [desc, intent-statement]

The goal is to centralise heating schedule management in Home Assistant, so the occupants interact with one familiar interface. Local/offline operation is a desired property. The existing Ramses project (`github.com/j3brns/Ramses`) has already proven the RAMSES-II communications path and test-world model; this phase extends it into HA Schedule helpers and zone setpoint dispatch. [intent-statement]

**Prime directive:** HA replaces the heating controls to the acceptance of everyone in the house. Stage 1 proves the system in test-world before any live changeover. [intent-statement, stakeholder-map]

## Feasibility & Risk Summary

**Overall verdict: Feasible.** The building blocks are in place. [feasibility-assessment]

| Risk | Level | Mitigation |
|---|---|---|
| RI-01: ramses_cc upstream API drift | Medium | Pin version on proven build; document upgrade test procedure |
| RI-02: MQTT test isolation not enforced | Low–Med | Define isolation mechanism (topic ACL or separate broker) before wiring automations |
| RI-03: HA version too old for Schedule helpers | Low | Verify HA ≥ 2023.6 at project start |
| RI-04: Manual transcription errors in zone schedules | Low | Cross-check fixture test: HA Schedule output vs. CM67z reference |

No regulatory constraints. No budget constraint. Single developer. [constraint-register, Q2, Q3]

## Scope Boundary

**Stage 1 (this initiative — test-world):** [scope-document]
- MQTT isolation mechanism defined
- ramses_cc version pinned
- HA Schedule helpers for all 4 zones (7-day, multi-period)
- Setpoint dispatch automations proven via MQTT test transport
- Deterministic pytest coverage; `check.ps1` passes
- Lovelace zone cards (temperature, target, mode, next change)
- Design supports up to ~8 zones (no hard-coded ceiling)

**Explicitly out of scope:** live RF, live MQTT, CM67z decommissioning, presence/weather integration, boiler control changes. [scope-document]

## Delivery Plan

8 backlog items (IB-01 to IB-08), all Must-have, sequenced risk-first: [intent-backlog]

1. **Phase A** (parallel): Define MQTT isolation · Pin ramses_cc · Confirm HA version
2. **Phase B**: Create HA Schedule helpers for all 4 zones
3. **Phase C**: Setpoint dispatch automations · pytest coverage · cross-check fixture
4. **Phase D**: Lovelace zone cards

**Team:** Single developer. No external resource needed. Working pace; no hard deadline. [Q3]

## Stakeholders

| Stakeholder | Role | Stage 1 involvement |
|---|---|---|
| Homeowner / developer | Decision-maker | Sole builder and approver for Stage 1 |
| Other household members | Acceptance gatekeepers | Stage 2 prerequisite — not involved in Stage 1 |

[stakeholder-map, Q1]

## Go / No-Go Recommendation

**Go.** [feasibility-assessment, scope-document]

All prerequisites are met: communications path proven, test infrastructure in place, risks documented with mitigations, scope clearly bounded. Stage 1 can proceed to Inception.

## Assumptions & Open Questions

- [assumption] No market research or mockups were produced; this brief is based on intent, feasibility, and scope artifacts only.
- [assumption] Household acceptance is a Stage 2 prerequisite, not Stage 1.
