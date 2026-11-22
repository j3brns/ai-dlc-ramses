## Sources

- [desc] Initial description: "Migrate Honeywell Evohome Heating zone programme controllers to home  assistant"
- [scope] Workflow-selected scope: `express`.

## Q1. [Developer] What existing systems must this integrate with?

A. Home Assistant (via ramses_cc custom component)
B. RAMSES-II RF devices (CM67z thermostats, TRVs, BDR91 boiler relay) via Elecram ESP32-C6 bridge
C. MQTT broker (local, on the same network)
D. All of the above
X. Other (please specify)

[Answer]: D — All of the above. Specifically: HA with ramses_cc (tracking upstream/latest), Elecram ESP32-C6 Sub-GHz bridge running ramses_esp over MQTT, CM67z Controllers 487 and 751, TRVs, and BDR91 boiler relay 642. The Ramses test model (Python, pytest, check.ps1) is also an integration point for test-world proofs. **Mode:** chat

## Q2. [Developer] Are there regulatory or compliance requirements?

A. Yes — data residency, GDPR, or similar
B. Yes — industry-specific (healthcare, financial, etc.)
C. No — purely a private home automation project
D. Not sure
X. Other (please specify)

[Answer]: C — No regulatory requirements. Private home automation. **Mode:** chat

## Q3. [Developer] What is the current tech stack and skill profile?

A. Python 3.11+, PowerShell, pytest, Home Assistant YAML, MQTT
B. Unknown / not yet decided
C. Other
X. Other (please specify)

[Answer]: A — Python 3.11+, PowerShell, pytest, check.ps1 gate, YAML (HA config and automations), MQTT. ramses_rf library for RAMSES-II protocol. Home Assistant (version not specified). **Mode:** chat

## Q4. [Product Owner] What are the budget and timeline constraints?

A. No external deadline; working at own pace
B. Hard deadline (name it)
C. Budget constraint (describe)
X. Other (please specify)

[Answer]: A — No external deadline. ASAP but at a sustainable working pace. No budget constraint stated. **Mode:** chat

## Q5. [Developer] Is ramses_cc version pinned or tracking upstream?

A. Pinned to a specific version
B. Tracking upstream (latest)
X. Other (please specify)

[Answer]: B — Tracking upstream (open/latest). **Mode:** chat

## Assumptions & Open Questions

- [assumption] No HACS or HA version constraint has been stated; the design assumes a current HA release compatible with ramses_cc upstream.
- [assumption] The MQTT broker is already running locally (Mosquitto or equivalent); no broker setup is in scope.
- [assumption] ramses_esp on the Elecram bridge is already configured and communicating with the MQTT broker.

## Assumption Confirmation

The following assumptions are carried in the artifacts:

**From feasibility-assessment.md:**
- No regulatory requirements apply.
- MQTT broker is already running locally; no broker setup is in scope.
- ramses_esp on the Elecram bridge is already configured and communicating with the broker.
- HA version supports the Schedule helper entity (2023.6+).
- No budget constraint; timeline is working pace with no external deadline.

**From constraint-register.md:**
- No external budget or timeline constraint.
- No GDPR, HIPAA, or other regulatory obligation applies.

**From raid-log.md:**
- No external blockers (change freeze, competing priorities) have been identified.

A. Accept assumptions
B. Convert to follow-up questions

[Answer]: A. Accept assumptions
