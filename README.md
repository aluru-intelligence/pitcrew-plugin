# PitCrew Plugin

Conversational workflow builder for the PitCrew compliance platform. Describe a workflow and the plugin produces a `board_draft` JSON spec you can run on PitCrew.

## Status

MVP — Builder only. Produces workflow JSON; execution against the PitCrew MCP server is a later phase.

## Architecture

- **Orchestrator** (`skills/orchestrator/`) — routes each user turn to a specialist subagent.
- **Subagents** (`agents/`) — one per worker lifted from `factory_v2`:
  - `plan-worker` — discovers tools, produces a plan spec
  - `edit-worker` — builds `board_draft` JSON from an approved plan
  - `chat-worker` — read-only questions about the current board
  - `knowledge-worker` — fills the `knowledge` block on a completed board
  - `config-worker` — fills the `configurations` block
- **Skills** (`skills/`) — detailed how-to each worker activates (`plan-mode`, `edit-mode`, etc.).
- **Tools catalog** (`tools/`) — one `.md` per available tool, with JSON frontmatter. Snapshot of `harness/tools/` from S3; `plan-worker` reads this to pick tools.
- **MCP** (`.mcp.json`) — points at the PitCrew MCP server for context tools (`list_policies`, `list_agents`). Streamable-http with OAuth 2.1 PKCE; the host handles the auth flow on first connect.

## Local dev (Claude Code)

For iteration, load the plugin for a single session with `--plugin-dir` — no marketplace, no install step:

```bash
# Validate first (optional but useful)
claude plugin validate /Users/sandeepbabu/projects/pitcrew-plugin

# Start a session with the plugin loaded
claude --plugin-dir /Users/sandeepbabu/projects/pitcrew-plugin
```

Then describe a workflow in chat and the orchestrator will dispatch to the right worker.

Inside the session, run `/plugin` to confirm the plugin loaded (check the **Installed** tab, and the **Errors** tab if anything is off). After editing any file in the plugin, run `/reload-plugins` to pick up the changes without restarting.

For always-on loading, symlink or copy this directory into `~/.claude/skills/` — Claude Code auto-loads any folder there that contains `.claude-plugin/plugin.json`.

For distribution (later), the plugin will be published through a marketplace — see https://code.claude.com/docs/en/plugins/marketplace-reference

## Repo layout

```
pitcrew-plugin/
├── .claude-plugin/plugin.json       # Claude Code manifest
├── .mcp.json                        # MCP server config
├── skills/                          # SKILL.md files (how workers should behave)
├── agents/                          # subagent definitions (dispatch entrypoints)
├── prompts/                         # system prompts referenced from skills/agents
├── tools/                           # tool catalog snapshot
└── README.md
```
