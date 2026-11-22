# Stakeholder Map — Ramses Heating Zone Programme Migration

## Key Stakeholders

| Stakeholder | Role | Primary Interest | Authority | Source |
|---|---|---|---|---|
| Homeowner / developer | Project owner, sole developer | Full control from HA; local/offline operation; clean, proven system | Decides scope and cutover | [Q1] |
| Other household members | End users of the heating interface | The interface is calm, clear, and dependable; heating works reliably; no surprises | Acceptance gatekeepers for live cutover | [Q3, Q5] |

## Decision-Makers vs. Influencers

| Person | Role | Decisions they own | Source |
|---|---|---|---|
| Homeowner / developer | Decision-maker | Scope, implementation approach, cutover timing, retirement of hardware | [Q1] |
| Other household members | Influencers / acceptance gatekeepers | Live cutover does not proceed without their acceptance (README prime directive) | [Q3] |

## Communication Requirements

| Requirement | Detail | Source |
|---|---|---|
| System transparency | The HA interface must show temperature, target, schedule, and next scheduled change for each zone at all times | [Q3, Q5] |
| Override visibility | Any temporary change must be visible in the UI; no hidden overrides | [Q5] |
| No live changes without evidence | Each behaviour must be proved with deterministic tests and bounded observation data before live deployment | [Q3, Q6] |
| Household acceptance | Live cutover requires the acceptance of all household members, not just technical sign-off | [Q3] |

## Assumptions & Open Questions

- [assumption] There is one developer/owner; no external team, no sprint ceremonies, no stakeholder reporting cadence beyond the household.
- [assumption] Other household members are non-technical users; the UI must work without explanation.
