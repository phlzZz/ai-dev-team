# AI Dev Team

Your own five-agent AI development team for Claude Code and Cowork. Each agent
owns one role in the build lifecycle, and a `dev-team` skill runs them as a
pipeline from idea to reviewed result.

These are the actual agent files in daily use — not a demo. They are plain
Markdown: read them, disagree with them, edit them.

## Overview

Instead of one generalist doing everything, work flows through five specialists:

1. **Architect** — turns your idea into a build plan and asks the key questions first.
2. **Designer** — turns the plan into UX flows, layout, design tokens, and an accessible, developer-ready handoff (skipped for headless/API-only work).
3. **Coder** — implements the code from the plan and the design handoff.
4. **Tester** — tests the coder's output and tries to break it; also checks accessibility.
5. **Manager** — reviews the combined work and flags issues before anything ships.

The team is tuned for **full-stack web** work (React + TypeScript + Tailwind on
the frontend, a Node/TypeScript or serverless API, Postgres/Supabase), but the
agents adapt to whatever a project already uses.

## Components

| Component | Name | Purpose |
|-----------|------|---------|
| Agent | `architect` | Idea → build plan; asks key questions first |
| Agent | `designer` | Plan → UX flows, design tokens, accessible handoff spec |
| Agent | `coder` | Implements the plan + design into working code |
| Agent | `tester` | Tests, breaks, and audits accessibility |
| Agent | `manager` | Reviews all work and flags blockers |
| Skill | `dev-team` | Orchestrates the five agents end to end on an existing project |

## Install

Clone the repo and point Claude Code at it as a plugin marketplace:

```
git clone https://github.com/phlzZz/ai-dev-team.git
```

Then in Claude Code:

```
/plugin marketplace add ./ai-dev-team
/plugin install ai-dev-team
```

**Or install the pieces by hand** — the agents and the skill are just Markdown
files, and Claude Code reads them from your config directory:

- copy `agents/*.md` into `~/.claude/agents/`
- copy `skills/dev-team/` into `~/.claude/skills/`

Restart Claude Code (or start a new session) and the five agents and the
`dev-team` skill are available.

## Usage

**Run the whole team on a feature or change in an existing project** — trigger
the `dev-team` skill: "run the AI dev team on this", "build this with the team",
"take this feature through the team". It runs
architect → designer → coder → tester → manager, looping the coder/tester until
tests pass and routing the manager's blockers back to whoever owns them.

**Starting a brand-new project?** Bootstrap it first — folder, git, deploy
target, and any design exploration — then run the `dev-team` skill as the build
phase. The team assumes there is something to build into.

**Call a single agent** when you only need one role: "use the architect to plan
this", "have the tester try to break this", "get the manager to review before I
merge".

The two natural check-in points are the architect's opening questions and the
manager's go/no-go verdict — the team pauses for you there when you're around,
and states its assumptions and proceeds when you're not.

## How handoffs work — the one rule worth keeping

**Every stage writes its output to a file, and the next stage is briefed with the
path.** The plan goes to `docs/plan.md`, the design handoff to
`docs/design-handoff.md`.

This is not bookkeeping. A subagent cannot see the orchestrating session's
history, so a brief that says "build it according to the architect's plan" hands
the coder *nothing* — it will quietly reconstruct what the brief happens to
mention and invent the rest. That failure is the reason the rule exists, and it
is written into every agent file.

## Setup

No configuration or environment variables required. The agents use the standard
Read/Write/Edit/Bash/Grep/Glob tools; the tester and coder run builds and tests
via Bash. If your project needs secrets (API keys, database URLs), the coder
uses environment variables and documents them in an `.env.example`.

### Optional design skills

The `designer` agent will consult these skills **if they are installed**, in this
order (earlier wins on conflict):

| Skill | What it contributes | Where it comes from |
|-------|--------------------|---------------------|
| `impeccable` | the design process spine | [`pbakaus/impeccable`](https://github.com/pbakaus/impeccable) |
| `ui-ux-pro-max` | style menu, palettes, font pairings | [`nextlevelbuilder/ui-ux-pro-max-skill`](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) |
| `huashu-design` | distinct design directions + critique rubric | [`alchaincyf/huashu-design`](https://github.com/alchaincyf/huashu-design) |
| `apple-design` | motion and interaction discipline | [`emilkowalski/skills`](https://github.com/emilkowalski/skills) |

**None of them ship with this plugin, and none of them are required.** With none
installed, the designer works from the principles written into its own file.

The designer also looks for an optional taste profile at
`~/.claude/skills/project-retro/TASTE.md` — a personal file recording which
design patterns you gravitate toward and which you reject. If it isn't there, the
designer says so in one line and designs from the brief.

## Customization

The default stack bias is full-stack web, but every agent is written to match an
existing project's conventions when one is present. To retune the team for a
different default stack, edit the "stack bias" / "craft standards" sections in
the files under `agents/`.

## A note on the dated notes in the files

Some agent files carry markers like `(Review 2026-07-29, Befund 1)`. Those are
provenance: each one marks a rule that was added because something went wrong in
a real build. They are kept deliberately — a rule with a scar attached is easier
to trust, and easier to delete when you decide it doesn't apply to you.

## License

MIT — see [LICENSE](LICENSE).
