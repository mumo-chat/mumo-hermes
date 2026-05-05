# mumo — Hermes Agent skill

**Multi-model deliberation for Hermes Agent.** When your local agent is decision-bound by its backbone model's confidence, mumo gives it on-demand access to a panel of frontier models — Claude, GPT, Gemini, Grok, Qwen, Kimi, GLM — for the hard calls.

For Claude Code, see [`mumo-chat/mumo-mcp`](https://github.com/mumo-chat/mumo-mcp). For Cursor, see [`mumo-chat/mumo-cursor`](https://github.com/mumo-chat/mumo-cursor). For VS Code, see [`mumo-chat/mumo-vscode`](https://github.com/mumo-chat/mumo-vscode).

## What's in the box

The repo root is the skill root — clone it directly into your Hermes skills directory and Hermes will discover `SKILL.md` at the canonical path.

- **`SKILL.md`** — the canonical skill teaching Hermes how to use mumo: when to invoke, the deliberation loop (create → wait → read → snippet → append/stop), how to read claim maps, snippet doctrine, and when to verify session creation.
- **`playbooks/`** — four cognitive-shape playbooks loaded on demand: `contested-decision`, `design-review`, `uncertainty-expansion`, `red-team`.
- **`reference/`** — five reference docs: `claim-maps`, `snippets`, `model-selection`, `synthesis`, `operating-notes`.
- **`config/mumo.yaml`** — the MCP server config to merge into `~/.hermes/config.yaml`.

## Install

### 1. Install the skill

Clone this repo directly into your Hermes skills directory:

```bash
git clone https://github.com/mumo-chat/mumo-hermes \
  ~/.hermes/skills/autonomous-ai-agents/mumo
```

That places `SKILL.md` at `~/.hermes/skills/autonomous-ai-agents/mumo/SKILL.md`, which is where Hermes' skill scanner expects it. If you prefer a different category, swap `autonomous-ai-agents` for whatever you use.

### 2. Get an API key

Sign up at [mumo.chat](https://mumo.chat) and create a platform key at [Settings → API Keys](https://mumo.chat/settings/api-keys). Keys start with `mmo_live_`.

### 3. Add the MCP server config

Open `~/.hermes/config.yaml`. Merge the contents of `config/mumo.yaml` from this repo under your `mcp_servers:` key, replacing `mmo_live_YOUR_KEY_HERE` with your actual key.

```yaml
mcp_servers:
  mumo:
    url: "https://mumo.chat/api/mcp"
    headers:
      Authorization: "Bearer mmo_live_YOUR_KEY_HERE"
    tools:
      include:
        - create_deliberation
        - wait_for_round
        - append_round
        - get_session
        - list_sessions
        - list_models
        - get_credit
      resources: false
      prompts: false
```

### 4. Restart Hermes

**Fully exit and restart Hermes.** The `/reload-mcp` slash command works for some installs but not all — restart is the canonical step.

After restart, the seven mumo tools surface as `mcp_mumo_create_deliberation`, `mcp_mumo_wait_for_round`, etc., and the skill is available to the agent.

## Using the panel

In a Hermes session, name `mumo` explicitly the first time so the agent reaches for the panel instead of answering directly:

> Ask mumo to compare Postgres and MongoDB for our event store given 50k events/day, a Postgres-experienced team, and a 3-month runway. What would we regret 6 months in?

Hermes calls `create_deliberation`, then `wait_for_round`. The completed round returns each model's prose plus a cross-model claim map showing where the panel agrees and where it splits. The skill teaches Hermes to read the claim map first, then react with typed snippets (KEEP / EXPLORE / CHALLENGE / CORE / SHIFT) and either append a follow-up round or stop and synthesize for you.

## When mumo is worth the latency tax

The skill encodes the trigger taxonomy in detail. In short:

- Architecture decisions with non-obvious tradeoffs
- Plan or design review before commitment
- Pre-launch pressure tests
- Stuck debugging after repeated failed attempts
- Pre-commit adversarial review on risky diffs (auth, payments, migrations)
- Memory/skill promotion gates
- Strategy questions with multiple defensible framings
- Explicit user requests

Skip mumo for routine refactors, formatting, syntax help, or anything where "just write a test" is cheaper than discussion.

## Verifying the call actually fired

Autonomous agents occasionally fabricate tool-call results — claiming a deliberation was sent when it wasn't. Real mumo session IDs are UUIDs (e.g. `2acdab34-2484-4bc5-a24f-bf917fe81477`). If a `create_deliberation` response doesn't contain a UUID-format `session_id`, the call did not happen. Verify by calling `list_sessions`. The skill teaches this discipline; this README is the user-facing reminder.

## Links

- Product — https://mumo.chat
- Install guide — https://mumo.chat/install/hermes
- MCP reference — https://mumo.chat/docs/mcp
- REST API — https://mumo.chat/docs/api
- Hermes Agent — https://hermes-agent.nousresearch.com
- Issues — https://github.com/mumo-chat/mumo-hermes/issues

## License

MIT
