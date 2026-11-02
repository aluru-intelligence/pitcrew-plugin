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

This repo is the **dev build** of the plugin (`pitcrew-dev`, pointing at `mcp.dev.gopitcrew.com`). It's a GitHub fork of the production repo (`aluru-intelligence/pitcrew-plugin`). Day-to-day work and product-team testing happens here. Shipping a verified change means opening a pull request from this fork into the production repo.

The three files below carry the dev-vs-prod configuration and intentionally differ between the two repos:

- `.mcp.json`
- `.claude-plugin/plugin.json`
- `.claude-plugin/marketplace.json`

These files should not be modified in this fork; they are the dev-side source of truth. When reviewing a dev → prod PR, the production repo keeps its own values for those three files.

Both the prod and dev builds can be installed side-by-side in Claude Code and Claude Desktop under different plugin names.

---

## License

MIT. See [LICENSE](./LICENSE).
