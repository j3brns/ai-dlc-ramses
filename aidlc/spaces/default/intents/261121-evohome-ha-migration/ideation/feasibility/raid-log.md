# RAID Log — Ramses Heating Zone Programme Migration

## Sources

- [desc] Initial description: "Migrate Honeywell Evohome Heating zone programme controllers to home  assistant"
- [scope] Workflow-selected scope: `express`.
- [Q1] Integration targets.
- [Q3] Tech stack.
- [Q4] Timeline.
- [Q5] ramses_cc version.

## Risks

| ID | Risk | Likelihood | Impact | Mitigation | Owner | Source |
|---|---|---|---|---|---|---|
| RI-01 | ramses_cc upstream API change breaks schedule dispatch automation | Medium | Medium — silent breakage on HA update | Pin ramses_cc version on proven build; document upgrade test procedure | Developer | [Q5] |
| RI-02 | MQTT test isolation not enforced at broker level — live device command possible | Low | High — unintended live heating change | Define and document isolation mechanism (topic ACL or separate broker) before wiring HA automations | Developer | [Q1] |
| RI-03 | HA version too old to support Schedule helper entity | Low | Medium — requires workaround | Verify HA version at project start; document minimum version (2023.6+) | Developer | [Q3] |
| RI-04 | Manual transcription of CM67z programmes introduces schedule errors | Low | Low — test-world only, no live impact in Stage 1 | Include cross-check fixture test comparing HA Schedule output to known CM67z reference | Developer | [Q1] |
| RI-05 | Household members do not accept system; live cutover delayed indefinitely | Low | Low for Stage 1; High for Stage 2 | Prove system in test-world first; demo to household before requesting acceptance | Developer | [desc] |

## Assumptions

| ID | Assumption | Validation approach | Status | Source |
|---|---|---|---|---|
| AS-01 | MQTT broker is already running locally on the same network as the Elecram bridge | Verify during infrastructure design | Open | [Q1] |
| AS-02 | ramses_esp on the Elecram bridge is already configured and communicating with the broker | Verify during infrastructure design | Open | [Q1] |
| AS-03 | HA version supports Schedule helper entity (2023.6+) | Check during environment setup | Open | [Q3] |
| AS-04 | No regulatory requirements apply | Confirmed by owner | Closed | [Q2] |
| AS-05 | Zone count will grow to ~8; design must not hard-code 4-zone ceiling | Parameterise zone config from the start | Open | [Q1] |
| AS-06 | No external budget or timeline constraint | Confirmed by owner | Closed | [Q4] |

## Issues

| ID | Issue | Severity | Status | Action | Source |
|---|---|---|---|---|---|
| IS-01 | MQTT isolation mechanism (topic namespace vs. broker ACL) not yet decided | Medium | Open | Define in MQTT transport contract during infrastructure design | [Q1] |
| IS-02 | HA version not yet confirmed as compatible with Schedule helper | Low | Open | Verify during environment setup | [Q3] |

## Dependencies

| ID | Dependency | Type | Status | Source |
|---|---|---|---|---|
| DE-01 | Elecram ESP32-C6 bridge + ramses_esp operational and communicating with MQTT broker | External hardware | Assumed in place | [Q1] |
| DE-02 | MQTT broker running locally | External service | Assumed in place | [Q1] |
| DE-03 | Home Assistant installed with ramses_cc custom component | External software | Assumed in place | [Q1] |
| DE-04 | ramses_rf Python library available in project environment | Library | In place (used by test-world model) | [Q3] |
| DE-05 | Existing Ramses test infrastructure (pytest, check.ps1) | Internal | In place | [Q3] |
| DE-06 | Existing weekly timer model and schedule contracts in Ramses repo | Internal | In place | [desc] |

## Assumptions & Open Questions

- [assumption] No external blockers (change freeze, competing priorities) have been identified. [Q4]
