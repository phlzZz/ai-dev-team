---
name: dev-team
description: >
  This skill should be used to run the five-agent AI dev team on an EXISTING
  project or codebase — a new feature, a non-trivial change, or a "build this
  properly" request where the user wants the whole team involved rather than a
  one-shot answer. Trigger on "run the dev team on this", "build this with the
  team", "kick off the dev team", "have the team implement X", or "take this
  feature through architect → designer → coder → tester → manager". For a
  brand-new project, first create a minimal working project, then run this
  skill as the build phase.
metadata:
  version: "0.3.1"
  author: "Phil (Brains & Pixels)"
---

# Dev Team — AI Dev Team Orchestration

Run a feature or non-trivial change on an existing project from idea to reviewed
result by coordinating five specialist agents in sequence: **architect →
designer → coder → tester → manager**. This skill is the conductor; the agents
do the work. (The designer stage is skipped for headless/API-only work with no
UI.)

## When to use

Trigger when the user wants the whole team involved on an **existing** project —
a new feature, a non-trivial change, or a "build this properly" request — rather
than a one-shot answer. For a tiny tweak, skip the ceremony and just do it.

**New project from scratch?** Create a minimal working project first, then use
this skill as the build phase. This package deliberately owns the development
workflow only; it does not include a project bootstrapper or deployment setup.

## The pipeline

Run the agents in order via the Agent/Task tool. Each stage hands off to the
next. Do not skip the architect and go straight to code — the plan is what keeps
the later stages honest.

### 1. Architect — plan first, ask questions first
Launch the `architect` agent with the user's idea. It must surface the key
questions before designing. **If a human is present, relay those questions and
wait for answers** before continuing. If the session is unattended, let the
architect state its assumptions explicitly and proceed. Output: an agreed build
plan (understanding, scope, architecture, data model, interfaces, build steps,
risks), **written to `docs/plan.md`** — later stages are briefed with that path,
never with "the plan from the architect" (agents can't see this session's
history; Review 2026-07-29, Befund 1).

### 2. Designer — design before building (skip only for headless/API-only work)
If the project has a user interface, launch the `designer` agent with the plan.
It produces the UX flows, screen inventory, layout, design tokens, component
specs (with every state), an accessibility spec mapped to WCAG, and key
microcopy — a developer-ready handoff, **written to `docs/design-handoff.md`**.
Accessibility is designed in here, so the Tester should find few a11y issues
later. Carry the plan AND this design handoff forward into coding **as file
paths** (`docs/plan.md`, `docs/design-handoff.md`). Skip this stage only for
CLI/API-only builds with no UI, and say so.

### 3. Scaffold (only if there's nothing to build into)
This skill usually runs on a project that **already exists** — in that case skip
this step entirely; the code, git repo, and dependencies are already there. Only
scaffold when you're genuinely starting from an empty directory:

- Create the project directory and initialize version control (`git init`).
- Scaffold the stack the architect chose (e.g. a Vite + React + TypeScript app,
  a Node/TypeScript API, or a Supabase-backed setup). Prefer the ecosystem's
  standard scaffolder over hand-rolling.
- Add `.gitignore`, a `README`, an `.env.example` for any secrets, and install
  dependencies.
- Confirm the empty skeleton builds/runs before handing off.

Keep scaffolding minimal — just enough for the coder to start on build step one.
When an external bootstrap process has already scaffolded the project, always
skip this step.

### 4. Coder — implement the plan and the design
Launch the `coder` agent with the **paths** `docs/plan.md` and
`docs/design-handoff.md` (never "the plan from earlier" — the coder can't see
this session's history; Review 2026-07-29, Befund 1) and the scaffolded
project. It implements the build steps in order, matching the design spec and the
project's conventions, handling real cases (loading, empty, error), and baking in
accessibility and basic security. Output: working code plus exact run
instructions and required env vars.

### 5. Tester — try to break it
Launch the `tester` agent on the coder's output. It writes and runs tests,
attacks edge cases and adversarial inputs, and audits accessibility (WCAG 2.1
AA). Finding failures is the goal. Output: a test report with a verdict and
failures grouped by severity.

**Loop:** if the tester finds Critical or Major failures, route them back to the
`coder` agent to fix, then re-run the `tester`. Repeat until the tester passes or
only accepted minor issues remain. Cap the loop at a sensible number of rounds
(default 3); if it still fails, escalate to the manager and the user rather than
looping forever.

### 6. Manager — review and flag
Launch the `manager` agent to review plan, design, code, and tests together. It
checks requirements were met, judges design, code, and test quality, and flags
blockers vs follow-ups. Output: a go / no-go verdict with specific blockers.

If the manager returns blockers, route each back to the responsible agent
(architect for plan gaps, designer for UX/visual/a11y-design gaps, coder for
code, tester for weak coverage), then re-review. Follow-ups can be tracked and
deferred with the user's agreement.

## Orchestration rules

- **Track it.** Maintain a task list with one task per stage so the user can see
  where things stand and what's left.
- **Hand off explicitly — as paths on disk.** Carry the plan AND the design
  handoff forward into every stage as the files `docs/plan.md` and
  `docs/design-handoff.md` — the coder builds against both, and the tester and
  manager judge against them. A brief that names a role instead of a path hands
  the agent nothing (Review 2026-07-29, Befund 1; the 29.07. incident).
- **Keep the human in the loop at the gates.** The architect's questions and the
  manager's go/no-go are the two moments to check in with the user when present.
- **Deliver as you go.** Share the plan when it's ready and the working result
  when it passes, rather than only at the very end.
- **Don't let roles blur.** The coder doesn't redesign, the tester doesn't
  rubber-stamp, the manager doesn't rewrite. If a stage is blocked, stop and say
  so instead of having one agent quietly do another's job.

## Output

At the end, give the user: the working project (with run instructions), a short
summary of what each agent contributed, the manager's verdict, and any tracked
follow-ups. Deliver code files and artifacts to the user rather than only
describing them.
