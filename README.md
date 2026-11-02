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

This repo uses a two-branch model:

- `master` is the dev build. Plugin name is `pitcrew-dev`, and `.mcp.json` points at `mcp.dev.gopitcrew.com`.
- `production` is the live build. Plugin name is `pitcrew`, and `.mcp.json` points at `mcp.gopitcrew.com`.

Feature branches merge into `master` first. Once a change is verified on dev, `master` is merged into `production` to ship it.

Three files are intentionally different between the two branches:

- `.mcp.json`
- `.claude-plugin/plugin.json`
- `.claude-plugin/marketplace.json`

To keep each branch's values intact during merges, run this one-time setup after cloning:

```sh
git config merge.ours.driver true
```

This registers the `ours` merge driver referenced in `.gitattributes`, so merging across branches preserves each branch's own copy of those three files instead of trying to combine them.

Both plugin names (`pitcrew-dev` and `pitcrew`) can be installed side-by-side in the same Claude Code client.

---

## License

MIT. See [LICENSE](./LICENSE).
