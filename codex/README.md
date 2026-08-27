# Codex installation

Copy `codex/skills/dev-team/` into your Codex skills directory, preserving its
`references/` subdirectory, then start a new Codex task in the existing project
you want to change.

Ask Codex to run the Dev Team on the change. The skill reads each bundled role
contract before its stage and uses delegated agents when that capability is
available. If delegation is unavailable, it preserves the same persisted
stages sequentially and reports the fallback.

The workflow requires named on-disk handoffs, one stateful-gate owner, one
integrator, no more than three repair cycles, and exactly one authoritative
full gate after the final repair. Project-specific acceptance criteria and
rendered evidence remain defined by the project.

Restart Codex or open a new task after installing or replacing the skill so the
new instructions are loaded.
