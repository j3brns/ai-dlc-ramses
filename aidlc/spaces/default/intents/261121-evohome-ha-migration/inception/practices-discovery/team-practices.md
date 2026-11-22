# Team Practices

Discovered practices for the Ramses heating zone migration project.
Single developer. Python 3.11+, PowerShell, pytest, check.ps1 quality gate.

---

## Way of Working

We work trunk-based on `master`. Each piece of work is squash-merged into `master` as one commit, keeping a linear history that maps cleanly to delivery planning. We revert commits when we need to undo shared history — no rebasing or force-pushing to published branches. Changes are kept small; dependencies between parallel pieces of work are recorded before parallel work begins.

- **Integration branch**: master
- **Merge strategy**: squash
- **History correction**: revert commits; do not rewrite published experiment history

---

## Walking Skeleton

We do not build a walking skeleton for this project. The existing Ramses repository already has a proven RF/MQTT communications path and an established test infrastructure (pytest, check.ps1, test-world integration checks using the `RAMSES/TEST/...` namespace). There is nothing to prove about end-to-end connectivity; the pieces already connect.

---

## Testing Posture

We write tests after implementation. Each testable layer is implemented first, then its pytest tests are written and run.

- **Methodology**: test-after
- **Methodology evidence**: The README mandates deterministic tests for state, schedules, overrides, encoding and transport changes. The existing test infrastructure is pytest-based with a check.ps1 gate running unit and test-world integration checks. No TDD or BDD tooling is configured.
- **Ordering**: Implement each applicable testable layer, then write and run that layer's tests.
- **Test strategy**: Minimal — one deterministic test per requirement, happy-path floor per component.
- **Regression requirement**: The existing test suite must remain green on every change.
- **Gate**: check.ps1 must pass. Test-world integration checks use the isolated `RAMSES/TEST/...` MQTT topic namespace.
- **Adversarial review**: Required for model, data, scenario, transport, CI or deployment changes (as stated in the README).

---

## Deployment

We deploy manually to a local Home Assistant instance. There is no CI/CD pipeline and no staging or production separation. The check.ps1 gate is the only automated quality gate: secrets scan, docs check, compile, unit checks, and test-world integration checks must all pass before a change is deployed.

- **Pipeline**: none — local check.ps1 gate only
- **Target**: local HA instance
- **Trigger**: manual, after check.ps1 passes and test-world integration checks are green
- **Environments**: single (local HA) — no staging/production separation

---

## Code Style

We defer to the project's existing linter and formatter configuration. No hand-written style rules are added beyond what the tooling enforces.

- **Python**: snake_case naming; Black formatter if configured in pyproject.toml; existing linter config governs
- **PowerShell**: consistent with existing project scripts
- **HA YAML**: standard Home Assistant conventions
- **No additional project-specific style rules stated**
