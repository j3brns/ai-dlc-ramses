# Decision Log — Ideation Phase

## Sources

- [desc] Initial description: "Migrate Honeywell Evohome Heating zone programme controllers to home  assistant"
- [scope] Workflow-selected scope: `express`.
- Upstream: `aidlc/spaces/default/intents/261121-evohome-ha-migration/ideation/intent-capture/intent-statement.md`
- Upstream: `aidlc/spaces/default/intents/261121-evohome-ha-migration/ideation/scope-definition/scope-document.md`
- Upstream: `aidlc/spaces/default/intents/261121-evohome-ha-migration/ideation/feasibility/feasibility-assessment.md`
- Upstream: `aidlc/spaces/default/intents/261121-evohome-ha-migration/ideation/feasibility/constraint-register.md`
- Upstream: `aidlc/spaces/default/intents/261121-evohome-ha-migration/ideation/scope-definition/intent-backlog.md`

## Decisions Made in Ideation

| ID | Decision | Rationale | Stage | Source |
|---|---|---|---|---|
| D-01 | The system is RAMSES-II based (CM67z thermostats), not Evohome | Hardware clarified in conversation; initial description was imprecise | intent-capture | [desc, Q2] |
| D-02 | Gateway: Elecram ESP32-C6 Sub-GHz bridge running ramses_esp over MQTT | Established hardware in the existing Ramses project | intent-capture | [Q2] |
| D-03 | Integration approach: build on ramses_cc HA custom component (tracking upstream) | Owner's stated approach; existing Ramses project already uses ramses_rf | intent-capture | [Q5] |
| D-04 | Stage 1 is test-world only — no live RF, no live MQTT, no live control | "Evidence then move" principle; "prove first" ethos from README | intent-capture | [Q3, desc] |
| D-05 | Phased delivery: Stage 1 test-world → Stage 2 live control → Stage 3 retire CM67z | Staged as confidence builds; no big-bang cutover | intent-capture | [Q3] |
| D-06 | Household acceptance is a Stage 2 prerequisite, not Stage 1 | Stage 1 is solo developer test-world work | approval-handoff | [Q1] |
| D-07 | No regulatory requirements; private home automation | Confirmed by owner | feasibility | [Q2] |
| D-08 | MQTT broker assumed already running locally; not in scope to set up | Confirmed by owner via chat | feasibility | [Q1] |
| D-09 | ramses_cc to be pinned on proven build (risk mitigation RI-01) | Tracking upstream introduces silent breakage risk | feasibility | [Q5, feasibility] |
| D-10 | MQTT test isolation mechanism to be defined before wiring HA automations (risk mitigation RI-02) | Topic-namespace vs. broker ACL — different implementation paths | feasibility | [Q1, feasibility] |
| D-11 | Stage 1 sequencing: risk-first (MQTT isolation + ramses_cc pin first) | Chosen by conductor per owner's instruction | scope-definition | [Q3] |
| D-12 | Zone count design ceiling: ~8 zones; no hard-coded 4-zone ceiling | 4 zones now, a few more later | scope-definition | [Q4, intent-statement] |
| D-13 | CM67z schedules manually transcribed into HA Schedule helpers for Stage 1 | Automated read-back not available; acceptable for test-world proof | scope-definition | [Q1, feasibility] |
| D-14 | All 8 backlog items (IB-01 to IB-08) are Must-have for Stage 1 | No nice-to-have deferral; full test-world proof is the Stage 1 bar | scope-definition | [Q2] |

## Assumptions & Open Questions

- [assumption] All decisions above are carried forward into Inception unchanged.
