# Discovered Rules

Hard constraints stated explicitly in the project's README and conventions.
Only rules with a clear human-stated basis appear here; inferences go in `evidence.md`.

---

## Mandated

- ALWAYS revert commits to undo shared history; do not rebase or force-push published experiment history.
- ALWAYS keep changes small; record dependencies before parallel work begins.
- ALWAYS add deterministic pytest tests for state, schedules, overrides, encoding and transport changes.
- ALWAYS add an adversarial review for model, data, scenario, transport, CI or deployment changes.
- ALWAYS run check.ps1 (secrets scan, docs check, compile, unit checks, test-world integration checks) before treating a change as done.
- ALWAYS use the isolated `RAMSES/TEST/...` MQTT topic namespace for test-world integration checks.

---

## Forbidden

- NEVER rewrite published experiment history (no rebase, no force-push to shared branches).
- NEVER deploy without check.ps1 passing in full.
