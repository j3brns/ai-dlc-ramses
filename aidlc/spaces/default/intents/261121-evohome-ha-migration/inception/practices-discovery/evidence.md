# Evidence

## Sources Inspected

- `aidlc/spaces/default/memory/org.md` — framework defaults for the five practice sections (Way of Working, Walking Skeleton, Testing Posture, Deployment, Code Style). Used as suggested defaults for a greenfield discovery run.
- `github.com/j3brns/Ramses` README — primary source of project conventions. Key extracts used below.
- Project context supplied in conversation brief: Python 3.11+, PowerShell, pytest, check.ps1 gate, single developer, personal home-automation project.

---

## What Was Inspected

### README.md (github.com/j3brns/Ramses)

The following specific statements from the README drove the practice decisions:

- "revert commits for shared history; do not rewrite published experiment history" → Way of Working: revert over rebase; Forbidden rule.
- "keep the change small; record dependencies before parallel work begins" → Way of Working: small changes, explicit dependency tracking; Mandated rule.
- "add deterministic tests for state, schedules, overrides, encoding and transport changes" → Testing Posture: test-after, deterministic pytest; Mandated rule.
- "add an adversarial review for model, data, scenario, transport, CI or deployment changes" → Testing Posture: adversarial review trigger; Mandated rule.
- pytest as test runner; check.ps1 gate running unit and test-world integration checks → Testing Posture gate and tooling.
- Test-world integration checks use isolated `RAMSES/TEST/...` MQTT topic namespace → Testing Posture, Mandated rule.
- No CI/CD pipeline; local check.ps1 is the gate → Deployment: manual, no pipeline.
- Deployment: manual to local HA instance; no staging/production separation → Deployment posture.
- Python 3.11+, PowerShell → language stack; Code Style defaults.

### org.md Defaults

The org.md defaults were consulted for each practice section and used as the starting point before project-specific evidence was applied:

- **Way of Working default**: trunk-based on `main`, squash-merge. Adjusted to `master` per the existing Ramses repo convention.
- **Walking Skeleton default**: scope-dependent. Decided No for this project (see below).
- **Testing Posture default**: test-after, Minimal strategy for the `express`/`classic` scope family. Confirmed appropriate.
- **Deployment default**: deploy on merge to staging; production gate manual. Adjusted: no pipeline, no staging, manual only.
- **Code Style default**: defer to project linter/formatter config. Confirmed appropriate.

---

## Interview Decisions

All five practice areas were resolved from the project context without requiring an interactive human session, as the brief supplied sufficient facts. Decisions recorded in `practices-discovery-questions.md`.

| Area | Decision | Basis |
|---|---|---|
| Way of Working | Trunk-based on `master`; squash-merge; revert history | README conventions + existing repo branch name |
| Walking Skeleton | No | Existing RF/MQTT path and test infra already proven end-to-end |
| Testing Posture | test-after; Minimal strategy; check.ps1 gate | README mandates + existing pytest/check.ps1 infrastructure |
| Deployment | Manual to local HA; check.ps1 must pass; no pipeline | README + project context |
| Code Style | Defer to pyproject.toml and existing configs | No additional rules stated in README |

---

## Inferences and Uncertainties

- **Branch name `master`**: The brief states "the README repo uses `master`". Org.md defaults name `main`. `master` is used here as stated. If the repo has since migrated to `main`, this should be updated at the affirmation gate.
- **Black formatter**: The brief notes "Black formatter if configured (check pyproject.toml)". Whether pyproject.toml actually configures Black is not confirmed from available evidence; the Code Style section defers to the file rather than asserting Black is active.
- **Adversarial review trigger**: The README's "add an adversarial review for model, data, scenario, transport, CI or deployment changes" is recorded as a Mandated rule. This applies to the AI-DLC review-cap setting for affected stages, not just the testing posture.
- **No collaborators**: The collaborators switch is off for this workflow run. No support-agent contribution files were produced. The lead draft stands as the practices without blind spoke review.

---

## Participants

- **Lead**: aidlc-pipeline-deploy-agent (greenfield practices discovery, single-developer project)
- **Support agents**: none (collaborators switch off)
- **Human decisions**: resolved from project context brief; no interactive interview session was needed
