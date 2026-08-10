# AI Dev Team

A five-role development workflow for **Claude Code** and **Codex**. It takes a
non-trivial change through **architect → designer → coder → tester → manager**
and keeps every handoff on disk, so later stages do not depend on hidden chat
context.

This package deliberately covers the build-and-review phase. Start it in an
existing project; project bootstrapping and deployment are not bundled.

## What it does

| Role | Responsibility | Required artifact |
| --- | --- | --- |
| Architect | Clarifies the work and makes a build plan | `docs/plan.md` |
| Designer | Defines UX, visual states, accessibility, and developer handoff | `docs/design-handoff.md` |
| Coder | Implements the agreed plan and design | Working code + run notes |
| Tester | Tries to break the result and checks accessibility | Test report |
| Manager | Reviews the full delivery | Go/no-go verdict |

The Designer is skipped only for headless or API-only work. Critical and major
test failures route back to the Coder before the Manager reviews the result.

## Claude Code

Clone this repository, then register and install it as a local plugin:

```text
/plugin marketplace add ./ai-dev-team
/plugin install ai-dev-team
```

Start a new session in an existing project and say: **“run the AI Dev Team on
this feature.”** Claude Code invokes the `dev-team` skill and its five agents.

## Codex

Copy `codex/skills/dev-team/` into your Codex skills directory, then start a
new Codex task inside the project you want to change. Say: **“run the AI Dev
Team on this feature.”**

The Codex adapter uses the same role instructions and disk-based handoffs. When
delegated-agent execution is available, it runs each role as a distinct agent;
otherwise it preserves the five stages sequentially and reports that fallback.
See [the Codex installation notes](codex/README.md).

## Usage

Use the full workflow for a feature, a non-trivial change, or work that needs a
genuine review gate. For a tiny isolated tweak, skip the ceremony.

The two natural check-in points are the Architect's questions and the Manager's
go/no-go verdict. In an unattended run, the Architect records assumptions in
the plan and the team continues.

## Principles

- Each stage writes its output to disk; the next stage receives the path.
- The Coder does not redesign; the Tester does not rubber-stamp; the Manager
  does not rewrite.
- A task is not complete merely because code was written: it must be tested and
  reviewed against the plan and design handoff.

## Customization

The team has a full-stack web bias—React, TypeScript, Tailwind, Node/serverless,
and Postgres/Supabase—but matches the conventions of an existing project rather
than forcing a stack. The shared role instructions are in `agents/`.

## License

MIT. See [LICENSE](LICENSE).
