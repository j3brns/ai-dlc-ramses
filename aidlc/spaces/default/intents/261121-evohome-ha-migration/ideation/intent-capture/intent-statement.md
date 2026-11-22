# Intent Statement — Ramses Heating Zone Programme Migration

## Sources

- [desc] Initial description: "Migrate Honeywell Evohome Heating zone programme controllers to home  assistant"
- [scope] Workflow-selected scope: `express`.

## Problem Statement

The house has four independently programmed heating zones spread across two Honeywell CM67z programmable room thermostats (Controller 487 and Controller 751, two zones each). Each zone's weekly heating programme is stored locally on the thermostat and operated in isolation. There is no unified view, no single place to adjust schedules, and no integration with other home systems. [Q1]

The goal is to centralise heating schedule management in Home Assistant, so the occupants interact with one familiar interface for the whole house. Local operation without internet dependency is a desired property. [Q1]

This is an extension of the existing Ramses project (`github.com/j3brns/Ramses`), which has already modelled the weekly timer, temporary/permanent overrides, and optimum start in test-world, and established the RAMSES-II gateway (Elecram ESP32-C6 + `ramses_esp` via MQTT). [Q5]

## Target Customer

A homeowner operating a multi-zone RAMSES-II heating system (CM67z programmable thermostats) who wants to manage all heating zones from Home Assistant without cloud dependency, and who requires other household members to find the interface calm, clear, and dependable before any live changeover. [Q1, Q3]

The system must be accepted by everyone who lives in the house — the README's prime directive. [Q5]

## Success Metrics

**Stage 1 (this work — test-world):** [Q3]

- All four existing CM67z zone schedules are represented as Home Assistant Schedule helpers with correct 7-day, multi-period programme data. [Q3, Q4]
- Deterministic tests demonstrate that the schedule layer sends the correct setpoint to each zone at the correct time via MQTT test transport (isolated `RAMSES/TEST/...` traffic). [Q3, Q5]
- A Lovelace card shows each zone's temperature, target, operating mode, and next scheduled switch. [Q3]
- The check gate (`check.ps1`) passes with no regressions. [Q5]

**Stage 2 (future — live cutover):** [Q3]

- HA can send live temperature setpoints to each zone via RAMSES-II; CM67z thermostats remain as fallback. [Q3, Q6]
- Boiler demand behaviour is proven unchanged. [Q6]

**Stage 3 (future — retirement):** [Q3]

- CM67z programmers are decommissioned; HA is the sole scheduling authority. [Q3]

## Initiative Trigger

The Ramses project is at the point where the MQTT test transport is the next implementation step. The schedule layer is modelled but not yet wired into HA entities and Lovelace. This is the natural next phase of the project: extend the proven test-world model to include HA Schedule helpers and the zone setpoint dispatch path. [Q5]

The trigger is project readiness, not external pressure. [Q1]

## Initial Scope Signal

**Workflow-selected scope:** `express` (custom plan: evohome-to-ha-migration). [scope]

**Product boundary for this version (what it does not do):** [Q6]

- No live RF transmission, no live control, no live MQTT — test-world only. [Q6]
- No decommissioning of CM67z thermostats. [Q6]
- No integration with presence detection, weather, or other HA devices. [Q6]
- No changes to the boiler interlock or boiler control hardware. [Q6]
- Lovelace UI uses standard cards; no custom frontend components. [Q6]

## Assumptions & Open Questions

- [assumption] The HA instance runs on a local server on the same network as the Elecram bridge.
- [assumption] The existing Ramses weekly timer and schedule contracts form the basis for HA Schedule helper definitions — no separate RAMSES-II schedule read-back step is needed for Stage 1.
- [assumption] CM67z current programmes will be manually transcribed into HA Schedule helpers for Stage 1 (automated read-back from thermostat is a future capability).
- [assumption] Zone count will grow to ~8; the Schedule helper design must not hard-code a 4-zone ceiling.
