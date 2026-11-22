# Practices Discovery Questions

## Q1: How does work get merged into the main line?

The existing Ramses repo uses a `master` branch. The README says to revert commits for shared history (no rewriting published experiment history) and to keep changes small.

Suggested default: trunk-based development on `master`; squash-merge each piece of work in as one commit; revert rather than rebase or force-push.

[Answer]: Trunk-based on `master`; squash-merge each piece of work; revert commits rather than rewriting history. Integration branch is `master`.

---

## Q2: Build a thin end-to-end slice first? A walking skeleton is a minimal version that runs the whole way through, built first to prove the pieces connect before the real features go in.

The Ramses project already has a proven RF/MQTT communications path and an established test infrastructure (pytest, check.ps1, test-world integration checks). There is no need to prove the pieces connect from scratch.

Suggested default: No walking skeleton — the project's existing infrastructure already demonstrates end-to-end connectivity.

[Answer]: No. The existing RF/MQTT communications path and test infrastructure are already proven. Building a skeleton from scratch is not the team's pattern here.

---

## Q3: How are tests written and when do they run?

The README says: "add deterministic tests for state, schedules, overrides, encoding and transport changes." Tests run through pytest; the check.ps1 gate runs unit and test-world integration checks. The test-world namespace is `RAMSES/TEST/...`.

Suggested default: test-after; write deterministic pytest tests once the implementation layer is in place; check.ps1 must pass before any change is considered done.

[Answer]: Test-after. Implement the testable layer first, then write and run its pytest tests. Use the Minimal strategy: one test per requirement, happy-path floor per component. check.ps1 must pass. Existing tests must stay green.

---

## Q4: How is a completed change deployed?

No CI/CD pipeline exists. Deployment is manual to a local Home Assistant instance. The check.ps1 gate is the only automated quality gate. There is no staging or production separation.

Suggested default: manual deploy to local HA instance once test-world proof is complete and check.ps1 passes; no automated pipeline.

[Answer]: Manual deployment to the local HA instance when check.ps1 passes and test-world integration checks are green. No CI pipeline. No staging/production separation.

---

## Q5: Any code-style rules to lock in?

Python: snake_case naming, Black formatter if configured in pyproject.toml. PowerShell: consistent with existing project scripts. HA YAML: standard HA conventions. No project-specific style rules beyond what the linter/formatter config enforces.

Suggested default: defer to project-level configurations; no additional hand-written rules.

[Answer]: Defer to pyproject.toml and existing linter/formatter configuration. Python — snake_case, Black if configured. PowerShell — consistent with existing scripts. HA YAML — standard HA conventions. No additional project-specific style rules stated.

---

## Consolidated Summary Confirmation

- **Way of Working:** Trunk-based on `master`; squash-merge each piece of work as one commit; revert rather than rebase or rewrite published history. Integration branch is `master`.
- **Walking Skeleton:** No — the existing RF/MQTT path and test infrastructure are already proven end-to-end.
- **Testing Posture:** Test-after methodology; implement the testable layer then write and run pytest tests; Minimal strategy (one test per requirement, happy-path floor per component); check.ps1 must pass; existing tests must stay green.
- **Deployment:** Manual to local HA instance; check.ps1 and test-world integration checks must pass; no CI pipeline; no staging/production separation.
- **Code Style:** Defer to pyproject.toml and existing configs; Python snake_case with Black if configured; PowerShell consistent with existing scripts; HA YAML standard conventions; no additional hand-written rules.

[Answer]: Looks correct
