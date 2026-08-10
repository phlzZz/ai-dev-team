---
name: dev-team
description: Run the AI Dev Team's architect-to-manager workflow for a non-trivial change in an existing project.
---

# AI Dev Team for Codex

Run a non-trivial feature or change through five distinct stages:
**architect → designer → coder → tester → manager**. This skill is the
Codex-native adapter for the shared role definitions in `references/`.

## Invocation

Use this skill when the user asks to run the dev team, take work through the
whole team, or requests architect-to-manager delivery on an existing project.
For a small isolated tweak, say that the full workflow is unnecessary and use
the smallest appropriate path instead.

## Execution

1. If the environment supports delegated agents, dispatch one specialist per
   stage in sequence. Give each specialist the relevant artifact *paths*, not a
   summary that relies on prior chat context.
2. If delegated agents are unavailable, run the same stages sequentially while
   preserving the role boundaries. Record this fallback in the final report.
3. The Architect writes `docs/plan.md`; the Designer writes
   `docs/design-handoff.md`. All later stages must read those files from disk.
4. Keep the user at the Architect questions and Manager go/no-go gates. For an
   unattended run, state assumptions in `docs/plan.md` and continue.
5. The Tester must try to break the implementation and run relevant checks.
   Route critical or major failures to the Coder, then re-test. Stop after
   three repair cycles and escalate unresolved failures to the Manager.
6. The Manager reviews the plan, design handoff, code, and test results; it
   returns a specific go/no-go verdict and lists blockers separately from
   accepted follow-ups.

## Role references

Read the matching role file before each stage:

- `references/architect.md`
- `references/designer.md` (skip only for headless or API-only work)
- `references/coder.md`
- `references/tester.md`
- `references/manager.md`

## Completion

Return the working project, run instructions, a concise stage-by-stage summary,
the Manager verdict, and any follow-ups. Never claim the workflow passed until
the project has been built/tested and the delivered result has been inspected.
