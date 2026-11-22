## Sources

- [desc] Initial description: "Migrate Honeywell Evohome Heating zone programme controllers to home  assistant"
- [scope] Workflow-selected scope: `express`.
- Upstream: `aidlc/spaces/default/intents/261121-evohome-ha-migration/ideation/intent-capture/intent-statement.md`
- Upstream: `aidlc/spaces/default/intents/261121-evohome-ha-migration/ideation/feasibility/feasibility-assessment.md`
- Upstream: `aidlc/spaces/default/intents/261121-evohome-ha-migration/ideation/feasibility/constraint-register.md`

## Q1. [Product Owner] What is the minimum viable scope that delivers value for Stage 1?

A. MQTT isolation contract defined + ramses_cc version pinned + all 4 zone schedules in HA Schedule helpers + setpoint dispatch proven in test-world
B. At least 1 zone schedule proven end-to-end (MQTT → HA Schedule helper → setpoint dispatch), then expand to remaining zones
C. All infrastructure decisions made (MQTT isolation, ramses_cc pin, HA version confirmed) but no schedule helpers yet
X. Other (please specify)

[Answer]: A — full 4-zone test-world proof is the Stage 1 goal. No partial delivery. **Mode:** chat

## Q2. [Product Owner] What capabilities are must-have vs. nice-to-have for Stage 1?

A. Must-have: MQTT test isolation definition, ramses_cc pin, 4-zone HA Schedule helpers, setpoint dispatch test, Lovelace zone card
B. Must-have: schedule helpers + dispatch test; nice-to-have: Lovelace card (can come after)
C. Must-have: schedule helpers only; MQTT isolation and ramses_cc pin are nice-to-have
X. Other (please specify)

[Answer]: A — all five items are must-have for Stage 1. **Mode:** chat

## Q3. [Developer] What is the sequencing preference?

A. Risk-first: MQTT isolation + ramses_cc pin first, then schedule layer, then Lovelace
B. Value-first: schedule layer first, then infrastructure decisions, then Lovelace
C. Dependency-first: follow the natural dependency chain
X. Other (please specify)

[Answer]: A — risk-first (chosen by conductor per owner's instruction). **Mode:** chat

## Q4. [Product Owner] Are there hard deadlines tied to specific capabilities?

A. No — working pace, no external deadline
B. Yes (describe)
X. Other (please specify)

[Answer]: A — no hard deadlines. **Mode:** chat

## Assumptions & Open Questions

- [assumption] Stage 1 is test-world only; no live RF or live MQTT traffic.
- [assumption] The sequencing is risk-first: infrastructure decisions before schedule layer build.

## Assumption Confirmation

The following assumptions are carried in the artifacts:

- Stage 1 is test-world only; no live RF or live MQTT traffic.
- The sequencing is risk-first: infrastructure decisions before schedule layer build.

A. Accept assumptions
B. Convert to follow-up questions

[Answer]: A. Accept assumptions
