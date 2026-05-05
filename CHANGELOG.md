# Changelog

## 0.1.0 — 2026-05-05

Initial release. Hermes Agent skill for mumo's MCP server.

- `skills/mumo/SKILL.md` — kernel teaching the deliberation loop, snippet doctrine, claim-map reading, when-to-invoke triggers, and the verification discipline (real session IDs are UUIDs; verify with `list_sessions` if uncertain).
- `skills/mumo/playbooks/` — four cognitive-shape playbooks: `contested-decision`, `design-review`, `uncertainty-expansion`, `red-team`.
- `skills/mumo/reference/` — five reference docs: `claim-maps`, `snippets`, `model-selection`, `synthesis`, `operating-notes`.
- `config/mumo.yaml` — MCP server entry to merge into `~/.hermes/config.yaml` with `tools.include` allowlist scoping mumo to its seven tools.
- README install steps: clone repo into `~/.hermes/skills/`, add YAML config, restart Hermes.

The skill content is shared with the v0.2.x mumo-mcp / mumo-cursor / mumo-vscode releases — same kernel and reference docs, adapted for Hermes' YAML-based MCP config and tool naming conventions.
