# AI Dev Team

A lean, artifact-first development workflow for **Claude Code** and **Codex**.
It takes a non-trivial change through
**architect → designer? → coder → tester → manager** while keeping every
handoff on disk instead of hidden in chat context.

The Designer runs for user-visible work and is skipped only when the accepted
plan explicitly marks the scope as headless or API-only.

## Roles

| Role | Owns |
| --- | --- |
| Architect | Scope, interfaces, risks, verification criteria, and the persisted plan |
| Designer | A persisted UX and interface handoff for user-visible work |
| Coder | Small inspectable implementation units and focused checks |
| Tester | Adversarial checks and a reproducible persisted report |
| Manager | Read-only final review and a specific go/no-go verdict |

## Workflow guarantees

- Every stage receives a named artifact path, not hidden conversation history.
- Tester starts only after an inspectable file, diff, rendered result, or named blocker exists.
- Two delegated checks without a new artifact or blocker trigger stop, narrowing, or reassignment.
- One named owner runs potentially colliding stateful test gates.
- One named integrator owns integration.
- Repair cycles are capped at three.
- Exactly one authoritative full gate runs after the final repair.
- Commit, push, publish, and installation remain separate human authorization gates.

Project-specific acceptance criteria stay in the project. For user-visible
work, require and inspect the rendered evidence appropriate to that project;
the public core does not hard-code repository paths or one universal browser
test matrix.

## Claude Code

Clone this repository, then register and install it as a local plugin:

```text
/plugin marketplace add ./ai-dev-team
/plugin install ai-dev-team
```

Start a new session in an existing project and ask it to run the Dev Team on a
non-trivial change.

## Codex

Copy `codex/skills/dev-team/` into your Codex skills directory, then start a
new task in the project you want to change. The skill reads the five role
contracts from its bundled `references/` directory before dispatching each
stage. See [the Codex installation notes](codex/README.md).

## Scope

This repository intentionally contains only the provider-neutral Dev-Team
core. It excludes private extensions, taste profiles, project context,
evidence archives, and model/provider routing.

Use the full workflow for a feature or risky change. For a genuinely trivial
one-line edit, the ceremony is usually unnecessary.

## License

MIT. See [LICENSE](LICENSE).
