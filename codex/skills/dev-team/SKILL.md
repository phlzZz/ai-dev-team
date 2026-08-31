---
name: dev-team
description: Run the lean five-role Dev-Team workflow for a non-trivial change in an existing project.
---

# Dev Team

Persist each handoff before the next stage begins. The default sequence is `architect -> designer? -> coder -> tester -> manager`; skip Designer only for an accepted headless or API-only scope.

Require an inspectable artifact before Tester starts. Two delegated checks without a new file, diff, rendered result, or named blocker trigger stop, narrowing, or reassignment. Name one stateful-test-gate owner and one integrator. Run potentially colliding gates serially, allow focused checks during implementation, cap repair cycles at three, and run exactly one authoritative full gate after the final repair.

For every user-visible surface, make accessibility a mandatory fail-closed acceptance gate. Browser-delivered work targets the applicable WCAG 2.2 AA criteria and records both repository-native automated analysis and manual evidence for keyboard-only operation, visible and logical focus, semantics and accessible names, forms and error states, contrast, and 200% zoom/reflow. Automated analysis alone never proves acceptance. Use the equivalent native accessibility checks for non-web interfaces. Missing, skipped, or non-reproducible required evidence requires Tester rejection and Manager no-go; only an explicitly accepted headless or API-only scope may mark the gate not applicable.

Commit, push, publish, and installation are separate human authorization gates.


## Codex stage references

Immediately before each stage that runs, read the matching relative role reference completely from disk:

- Before the Architect stage: read `references/architect.md`.
- Before the Designer stage: read `references/designer.md`.
- Before the Coder stage: read `references/coder.md`.
- Before the Tester stage: read `references/tester.md`.
- Before the Manager stage: read `references/manager.md`.

A stage must not begin until its role reference has been read.
