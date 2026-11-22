# Phase Verification — Ideation → Inception

**Date:** 2026-11-21

## Coverage Check

### Intent → Scope consistency

| Intent item | Scope coverage | Status |
|---|---|---|
| Centralise heating schedule management in HA | IB-04 (Schedule helpers), IB-05 (dispatch) | ✅ Covered |
| Local/offline operation (nice-to-have) | TC-02 (RAMSES-II, no cloud) | ✅ Covered |
| 4 zones, support growth to ~8 | TC-07, D-12 | ✅ Covered |
| Prove in test-world before live | TC-01, D-04 | ✅ Covered |
| Phased retirement of CM67z | Stage 2/3 (explicitly deferred) | ✅ Deferred appropriately |
| Household acceptance before live | OC-01, D-06 | ✅ Stage 2 gate |

### Scope → Backlog consistency

| Scope item | Backlog coverage |
|---|---|
| MQTT isolation mechanism defined | IB-01 |
| ramses_cc version pinned | IB-02 |
| HA version confirmed | IB-03 |
| HA Schedule helpers (4 zones) | IB-04 |
| Setpoint dispatch automations | IB-05 |
| Deterministic pytest tests | IB-06 |
| Cross-check fixture test | IB-07 |
| Lovelace zone cards | IB-08 |

All 8 scope items map 1:1 to backlog items. ✅

### Feasibility backing for scope items

| Scope item | Feasibility backing |
|---|---|
| MQTT isolation | RI-02 mitigation — explicitly required before wiring |
| ramses_cc pin | RI-01 mitigation — explicitly required before dispatch |
| HA version ≥ 2023.6 | AS-03 — assumption to verify |
| Schedule helpers (7-day) | HA native capability confirmed |
| Setpoint dispatch | RAMSES-II path established (Elecram bridge + ramses_esp) |
| pytest tests | Python + pytest stack in place |
| Lovelace cards | Standard HA capability |

All scope items have feasibility backing. ✅

## Warnings

- IS-01 (MQTT isolation mechanism not yet decided) and IS-02 (HA version not confirmed) are open issues carried into Inception — they are the first Phase A items and must be resolved before the schedule layer is built.

## Result

**PASS.** Ideation artifacts are consistent and complete. Ready to proceed to Inception.

- [x] Intent → Scope → Backlog chain is consistent
- [x] All scope items have feasibility backing
- [x] No contradictions between artifacts
- [x] Open issues documented and sequenced first in delivery plan
