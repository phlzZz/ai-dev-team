# Codex installation

Copy `codex/skills/dev-team/` into your Codex skills directory, then start a
new Codex task in the project you want to change. Ask: “run the AI Dev Team on
this feature.”

The skill uses the same five roles and disk-based handoffs as the Claude Code
release. It uses Codex's available delegated-agent capability when present and
otherwise keeps the same stages sequentially in one task, reporting that
fallback at the end.
