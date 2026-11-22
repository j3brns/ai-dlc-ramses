## Sources

- [desc] Initial description: "Migrate Honeywell Evohome Heating zone programme controllers to home  assistant"
- [scope] Workflow-selected scope: `express`.

## Q1. [Product Owner] What business problem are we solving?

Honeywell Evohome allows heating zones to be programmed independently. This work aims to replicate or replace that scheduling behaviour inside Home Assistant. What is the core problem you want solved?

A. The Evohome app/controller is being replaced and HA must become the scheduling hub  
B. I want a single smart home platform (HA) to manage everything, including heating  
C. The Evohome schedule programming is too limited and I want HA automation flexibility  
D. I want remote access and integration with other HA devices/automations  
E. Other — Evohome hardware stays but I want HA to control the schedules  
X. Other (please specify)

[Answer]: B — centralisation and ease; local/offline operation is a nice-to-have, not mandatory. **Mode:** chat

## Q2. [Product Owner] Which Evohome setup do you have?

Understanding your hardware helps determine the integration approach.

A. Evohome with the standard Honeywell web account (cloud API access via evohomeclient2)  
B. Evohome running fully locally (no cloud, local REST API only)  
C. Evohome controlled via the Evohome WiFi Gateway (HGI80 / USB gateway)  
D. Not sure — I have an Evohome controller and zone thermostats but I don't know the connectivity  
X. Other (please specify)

[Answer]: X — Not Evohome. Two CM67z programmable room thermostats (RAMSES-II protocol), 2 zones each (4 zones total). Gateway is an Elecram ESP32-C6 Sub-GHz bridge running ramses_esp over MQTT. Two controllers: 487 (Kitchen, EJMB+RDGB) and 751 (Lounge, CMQ JGB). Boiler demand via thermostat 226 → BDR91 642, interlocked but separate. **Mode:** chat

## Q3. [Product Owner] What does success look like?

Which of these best describes a successful outcome for this migration?

A. All existing Evohome zone schedules are running in Home Assistant with no manual re-entry  
B. HA can send temperature setpoints to each zone on a schedule, Evohome hardware stays as the heat source controller  
C. HA fully replaces Evohome for scheduling; Evohome controller is decommissioned  
D. I can edit heating schedules from the HA dashboard and they take effect immediately  
X. Other (please specify)

[Answer]: B and D combined, with phased retirement as the end goal. Stage 1 (this work): read CM67z schedules, represent them in HA Schedule helpers, prove correct setpoint dispatch via MQTT test transport. Stage 2 (future): live control. Stage 3 (future): retire CM67z programmers. All within the "evidence then move" principle. **Mode:** chat

## Q4. [Product Owner] Which zones and schedule types are in scope?

How many heating zones do you have, and what kinds of schedule do they run?

A. 1–4 zones, simple weekday/weekend schedules (same set/setback each day)  
B. 1–4 zones, full 7-day individual schedules  
C. 5–12 zones, full 7-day individual schedules  
D. 12+ zones, complex multi-period schedules (3+ time slots per day per zone)  
X. Other (please specify)

[Answer]: B — 4 zones now (Controller 487: Kitchen, EJMB+RDGB; Controller 751: Lounge, CMQ JGB), a few more later. Full 7-day individual schedules per zone. Design must handle zone count growth. **Mode:** chat

## Q5. [Product Owner] What is already in place?

Which of the following do you currently have set up?

A. Home Assistant installed and running (I just need to add the Evohome integration and automations)  
B. Home Assistant installed but not yet connected to any heating devices  
C. Home Assistant not yet installed — this migration is the first use case  
D. I have HACS (Home Assistant Community Store) available  
X. Other (please specify) — select all that apply by writing the letters

[Answer]: X — Existing project (Ramses repo, github.com/j3brns/Ramses). Gateway (Elecram ESP32-C6 + ramses_esp) in place. Weekly timer model, temporary/permanent override contracts, optimum start model core already built in test-world. MQTT test transport is the next implementation step. HA entities and Lovelace not yet wired. Live control deliberately not started. Python 3.11+, PowerShell, pytest, check.ps1 gate. **Mode:** chat

## Q6. [Product Owner] What must this first version NOT do?

The plan is scoped as a migration. Which of these are out of scope for now?

A. No new HA automations beyond replicating the existing Evohome schedules  
B. Do not decommission the Evohome controller (keep it as a fallback)  
C. Do not integrate with other HA devices (e.g. presence detection, weather) in this phase  
D. Do not build a custom HA dashboard; use the default Lovelace cards  
X. Other (please specify)

[Answer]: A, B, C — stay in test-world (no live RF, no live control, no live MQTT), no CM67z decommissioning, no presence/weather integration yet. Keep Lovelace simple per README prime directive. No live boiler control changes. **Mode:** chat

## Q7. [Developer] How should the migration be done?

How would you prefer the zone schedules to be migrated into Home Assistant?

A. Export schedules from Evohome (via the API or manually) and import them as HA automation YAML  
B. Keep Evohome as the schedule store; have HA read and apply the schedule data via the integration  
C. Manually re-enter the schedules in HA using the UI scheduler or automation editor  
D. Use a script/tool to generate the HA automation files automatically from the Evohome API  
X. Other (please specify)

[Answer]: X — Schedules are stored in the CM67z thermostats (RAMSES-II). The existing Ramses project already has the weekly timer model and schedule contracts. This work extends that: read current CM67z schedule programmes via RAMSES-II/MQTT, represent them as HA Schedule helpers, prove correct setpoint dispatch in test-world (isolated RAMSES/TEST/... MQTT traffic). No Evohome API involved. **Mode:** chat

## Assumptions & Open Questions

- [assumption] The HA instance is or will be running on a local server on the same network as the Elecram bridge.
- [assumption] The existing Ramses weekly timer and schedule contracts (already in the repo) form the basis for the HA Schedule helper definitions — no separate schedule export/import step is needed.
- [assumption] The CM67z thermostats' current programmed schedules will be manually transcribed into HA Schedule helpers for this first version (reading schedules from the thermostat via RAMSES-II is a future capability).
- [assumption] "A few more zones later" means up to ~8 zones total; the Schedule helper design should not hard-code a 4-zone ceiling.

## Assumption Confirmation

The following assumptions are carried in the artifacts. Please confirm how to handle them:

**From intent-statement.md:**
- The HA instance runs on a local server on the same network as the Elecram bridge.
- The existing Ramses weekly timer and schedule contracts form the basis for HA Schedule helper definitions — no separate RAMSES-II schedule read-back step is needed for Stage 1.
- CM67z current programmes will be manually transcribed into HA Schedule helpers for Stage 1 (automated read-back from thermostat is a future capability).
- Zone count will grow to ~8; the Schedule helper design must not hard-code a 4-zone ceiling.

**From stakeholder-map.md:**
- There is one developer/owner; no external team, no sprint ceremonies, no stakeholder reporting cadence beyond the household.
- Other household members are non-technical users; the UI must work without explanation.

A. Accept assumptions
B. Convert to follow-up questions

[Answer]: A. Accept assumptions
