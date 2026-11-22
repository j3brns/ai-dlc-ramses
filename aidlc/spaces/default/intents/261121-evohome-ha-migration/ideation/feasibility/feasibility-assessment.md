# Feasibility Assessment — Ramses Heating Zone Programme Migration

## Sources

- [desc] Initial description: "Migrate Honeywell Evohome Heating zone programme controllers to home  assistant"
- [scope] Workflow-selected scope: `express`.
- [Q1] Integration targets confirmed: HA + ramses_cc, Elecram bridge + ramses_esp, MQTT broker, CM67z controllers, TRVs, BDR91.
- [Q2] No regulatory requirements.
- [Q3] Tech stack: Python 3.11+, PowerShell, pytest, HA YAML, MQTT, ramses_rf.
- [Q4] No external deadline; working pace.
- [Q5] ramses_cc tracking upstream.
- Upstream: `aidlc/spaces/default/intents/261121-evohome-ha-migration/ideation/intent-capture/intent-statement.md`

## Overall Verdict

**Feasible.** The technical building blocks exist and are already partially assembled in the Ramses project. The main uncertainties are around the ramses_cc schedule-read API surface (tracking upstream introduces drift risk) and the MQTT isolation contract for test-world proofs. Neither is a blocker; both are managed risks. [Q1, Q5]

## Technical Feasibility

### What is already proven

- RAMSES-II RF communication path: Elecram ESP32-C6 → ramses_esp → MQTT → HA is established. [Q1]
- Weekly timer model, temporary/permanent override contracts, and optimum start model core are implemented and tested in the Ramses test-world. [desc]
- Python test infrastructure (pytest, check.ps1 gate) is in place. [Q3]
- The project's "prove first" discipline (isolated RAMSES/TEST/... MQTT traffic, deterministic tests, bounded observation data) is documented and operative. [desc]

### What is new in this phase

The schedule layer must be extended from the existing test-world model into Home Assistant entities: [Q1, Q3]

1. **HA Schedule helpers** — represent each zone's 7-day programme as an HA `input_datetime` or Schedule helper entity.
2. **Zone setpoint dispatch** — an HA automation (or ramses_cc service call) sends the correct target temperature to each zone's Climate entity at the scheduled time.
3. **Lovelace visibility** — a card per zone showing temperature, target, operating mode, and next scheduled switch.
4. **Test-world proof** — MQTT traffic remains isolated to `RAMSES/TEST/...` topics; deterministic pytest tests cover the schedule → setpoint dispatch path.

### Integration analysis

| Integration point | Status | Risk |
|---|---|---|
| Elecram bridge + ramses_esp → MQTT | Established | Low — hardware in place [Q1] |
| MQTT broker (local) | Assumed running | Low — standard setup [Q1] |
| ramses_cc HA custom component | Tracking upstream | Medium — API surface may change without notice [Q5] |
| HA Schedule helpers / automations | To be built | Low — well-documented HA pattern [Q3] |
| ramses_rf Python library | Used in test-world model | Low — already integrated [Q3] |
| CM67z thermostat schedule read-back | Out of scope (Stage 1) | N/A — manual transcription for Stage 1 [Q1] |

### Technical risks

**R1 — ramses_cc upstream drift (Medium)**
ramses_cc is tracking upstream. The Climate entity API, service call signatures, and MQTT topic structure may change between HA/ramses_cc releases. The schedule dispatch automation may break silently on update. [Q5]
Mitigation: pin the ramses_cc version at the point the schedule dispatch is proven; document the pinned version and the upgrade test procedure.

**R2 — MQTT test isolation enforcement (Low–Medium)**
The project requires MQTT traffic to stay on isolated `RAMSES/TEST/...` topics during test-world operation. The enforcement mechanism (topic-namespace only vs. broker-level ACL) is not yet specified. A mis-routed command could reach live devices. [Q1]
Mitigation: define the isolation mechanism explicitly in the MQTT transport contract before wiring HA automations to the broker.

**R3 — HA Schedule helper expressiveness (Low)**
The CM67z supports multi-period 7-day schedules. HA's built-in Schedule helper (introduced in HA 2023.6) supports this natively. If the HA version in use predates this, a workaround is needed. [Q1, Q3]
Mitigation: verify HA version at project start; document the minimum HA version required.

**R4 — Manual transcription accuracy (Low)**
Stage 1 relies on manually transcribing CM67z programmes into HA Schedule helpers. Transcription errors will silently produce incorrect heating schedules. [Q1]
Mitigation: include a cross-check step in the test suite that compares the HA Schedule helper output against a reference fixture derived from the known CM67z programme.

## Assumptions & Open Questions

- [assumption] No regulatory requirements apply. [Q2]
- [assumption] MQTT broker is already running locally; no broker setup is in scope.
- [assumption] ramses_esp on the Elecram bridge is already configured and communicating with the broker.
- [assumption] HA version supports the Schedule helper entity (2023.6+).
- [assumption] No budget constraint; timeline is working pace with no external deadline. [Q4]
