---
name: dev-team
description: Run the lean five-role Dev-Team workflow for a non-trivial change in an existing project.
---

# Dev Team

Persist each handoff before the next stage begins. The default sequence is `architect -> designer? -> coder -> tester -> manager`; skip Designer only for an accepted headless or API-only scope.

Require an inspectable artifact before Tester starts. Two delegated checks without a new file, diff, rendered result, or named blocker trigger stop, narrowing, or reassignment. Name one stateful-test-gate owner and one integrator. Run potentially colliding gates serially, allow focused checks during implementation, cap repair cycles at three, and run exactly one authoritative full gate after the final repair.

Commit, push, publish, and installation are separate human authorization gates.

