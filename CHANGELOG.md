# Changelog

## 0.1.3 — 2026-05-05

Frontmatter aligned with HermesHub conventions after sampling existing hub skills (notion-integration, api-builder, agent-hardening, synapse-swarm). v0.1.2 used the generic agentskills.io shape; this matches what the hub actually ships.

- **`version`** now string-quoted (`"0.1.3"`).
- **`author`** moved under `metadata.author` (hub convention; top-level `author` is non-canonical for hermeshub).
- **`compatibility`** field added — pointer to `mumo.chat/settings/api-keys` for the platform key.
- **`metadata.hermes.category: agents`** — same bucket as synapse-swarm and paperclip (multi-agent collaboration).
- **Tags** rewritten in flow style to match hub convention.

## 0.1.2 — 2026-05-05

Frontmatter cleanup ahead of HermesHub submission. No body changes.

- **Frontmatter completed** — added `version`, `author`, `license`, and `metadata.hermes.tags` (deliberation, multi-model, mcp, decision-support, ai-tooling) per Hermes' creating-skills guide. Brings v0.1.x in line with hub frontmatter conventions.

## 0.1.1 — 2026-05-05

Skill refinements from first real Hermes deliberation, plus a layout fix. The v0.1.0 skill produced clean end-to-end behavior (real tool calls, auto-chained wait, confident synthesis); these edits sharpen the framing.

- **Repo restructured** — root is now the skill root (was `skills/mumo/`). Cloning the repo into `~/.hermes/skills/<category>/mumo/` now places `SKILL.md` at the canonical path Hermes' scanner expects, instead of one level too deep.
- **"When to use" reframed** around the three conditions Hermes' own synthesis identified: wide solution space + hidden failure space; medium-high confidence + anchoring risk; irreversible consequences. The longer trigger taxonomy stays as supporting detail. Added the "cognitive load balancer" framing to position mumo against your reasoning, not as a replacement for it.
- **Long-wait guidance** in basic loop step 3: 15–120s is normal, 60+ seconds isn't a failure signal. Tell the user upfront so the wait doesn't feel broken.
- **Recovery section** for lost session context: use `list_sessions` to find your latest session by prompt match, then `get_session` or `wait_for_round` instead of starting a duplicate deliberation.
- **Terminology section** distinguishing panel/models/participants (what mumo invokes on its backend) from subagent (your own delegation infrastructure). Hermes' first synthesis conflated the two; this section heads off the category error in future synthesis.

## 0.1.0 — 2026-05-05

Initial release. Hermes Agent skill for mumo's MCP server.

- `skills/mumo/SKILL.md` — kernel teaching the deliberation loop, snippet doctrine, claim-map reading, when-to-invoke triggers, and the verification discipline (real session IDs are UUIDs; verify with `list_sessions` if uncertain).
- `skills/mumo/playbooks/` — four cognitive-shape playbooks: `contested-decision`, `design-review`, `uncertainty-expansion`, `red-team`.
- `skills/mumo/reference/` — five reference docs: `claim-maps`, `snippets`, `model-selection`, `synthesis`, `operating-notes`.
- `config/mumo.yaml` — MCP server entry to merge into `~/.hermes/config.yaml` with `tools.include` allowlist scoping mumo to its seven tools.
- README install steps: clone repo into `~/.hermes/skills/`, add YAML config, restart Hermes.

Derived from the shared v0.2.x skill kernel that originated in mumo-mcp and mumo-cursor, adapted for Hermes' YAML-based MCP config and tool naming conventions. Note: mumo-vscode no longer ships `skills/` in the published `.vsix` (excluded as of v0.3.0), since VS Code's Copilot doesn't consume `SKILL.md` at runtime — the kernel is shared in spirit, not in a runtime sense.
