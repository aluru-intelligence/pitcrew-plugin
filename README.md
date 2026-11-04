# PitCrew

**Build and run AI compliance agents by chatting.** Describe an agent in plain English, review the plan, and PitCrew publishes it to your workspace and runs it on your documents, without leaving the conversation.

Built for compliance teams, financial advisors, and operations teams who want to automate repeatable document work without hand-building workflows.

---

## What you can do with it

Ask Claude in plain English and the plugin takes over:

- *"Build me an agent that summarises Form ADV filings and emails the key risks to compliance@myfirm.com."*
- *"Create a workflow that reads a client's quarterly statement, extracts the top three holdings, and flags anything over a 10% concentration."*
- *"Build an agent that takes a batch of billing exports, checks each row against our fee schedule, and reports mismatches."*
- *"Run my 'Policy Diff Checker' agent on the three PDFs I just uploaded to the Q4 Compliance collection."*

Behind the scenes, the plugin coordinates a small team of specialists (a planner, an agent builder, a knowledge writer, and a settings generator) to turn your description into a complete, runnable PitCrew agent. You review the plan before anything is built; you review the finished agent before anything runs.

---

## How it works

1. **Describe.** Tell Claude what you want the agent to do.
2. **Plan.** The planner proposes a step-by-step plan and asks for your approval.
3. **Build.** Once you approve, the agent builder produces a complete workflow specification (swimlanes, tasks, inputs, settings) ready to run.
4. **Publish.** The spec is published to your PitCrew workspace as a new agent.
5. **Run.** Provide inputs (or attach documents from a collection you own) and the agent executes on PitCrew. You see the final report inline in the chat.

Every step runs inside one conversation. No tabs, no form-filling, no copy-paste.

---

## Requirements

- A PitCrew account at [gopitcrew.com](https://www.gopitcrew.com/). The first time you run an agent, Claude prompts you to connect; a one-time sign-in handles it.
- Claude on any supported surface: claude.ai, Claude Desktop, Claude Code, Cowork.

---

## Install

PitCrew ships as two builds: a **production** build and an internal **dev** build. Each lives in its own GitHub repo and installs as a separate marketplace, so both can coexist in the same Claude client.

| Build | Marketplace source | Plugin name |
|---|---|---|
| Production | `pitcrew-agents/pitcrew-plugin` | `pitcrew` |
| Dev (internal testing) | `pitcrew-agents/pitcrew-plugin-dev` | `pitcrew-dev` |

### In Claude Desktop or on claude.ai

1. Open **Customize → Plugins → + Add → Add marketplace**.
2. Paste the marketplace source for the build you want (see table above).
3. Click **Sync**. The plugin appears in the marketplace list.
4. Open the plugin's card and click **Add** to enable it on your account.

### In Claude Code

Production:

```sh
/plugin marketplace add pitcrew-agents/pitcrew-plugin
/plugin install pitcrew@pitcrew-plugin
```

Dev:

```sh
/plugin marketplace add pitcrew-agents/pitcrew-plugin-dev
/plugin install pitcrew-dev@pitcrew-plugin-dev
```

The first time you run a PitCrew tool, Claude will prompt you to sign in to your PitCrew account.

---

## Privacy and data handling

PitCrew is purpose-built for regulated workflows. Documents you process through the plugin flow to your PitCrew workspace, where your organisation's data retention and access controls apply.

- **Privacy policy:** [gopitcrew.com/privacy](https://www.gopitcrew.com/privacy)
- **Security overview:** [gopitcrew.com/security](https://www.gopitcrew.com/security)
- **Terms of service:** [gopitcrew.com/terms](https://www.gopitcrew.com/terms)

---

## Support

- **Email:** [support@gopitcrew.com](mailto:support@gopitcrew.com)
- **Homepage:** [gopitcrew.com](https://www.gopitcrew.com/)

---

## Development

This repo is the **production build** of the plugin (`PitCrew MCP`, pointing at `mcp.gopitcrew.com`). A separate dev-build repo is maintained as a GitHub fork of this one, and shipping a change means opening a pull request from the fork into this repo.

The three files below carry the dev-vs-prod configuration and intentionally differ between the two repos:

- `.mcp.json`
- `.claude-plugin/plugin.json`
- `.claude-plugin/marketplace.json`

These files are the dev-side source of truth and should not be modified here except as a deliberate dev configuration change. When reviewing a dev → prod PR, the production repo keeps its own values for those three files.

Both the prod and dev builds can be installed side-by-side in Claude Code and Claude Desktop under different plugin names.

---

## License

MIT. See [LICENSE](./LICENSE).
