# Tafla plugin

[Tafla](https://www.tafla.is) shows the working behind a number as a walkthrough: the givens, each step with its formula, the result circled, and captions or a voice that talk the reader through it. The calculation stays live, so the reader can drag an input and watch every step recalculate.

This repo is the Claude Code plugin. It adds the Tafla MCP server, which has one tool, `explain_calculation`, and a skill that says when to hand a calculation to it.

## Install

```sh
claude plugin marketplace add hoddmachine/tafla-plugin
claude plugin install tafla@tafla
```

Then, in a Claude Code session, type `/mcp`, pick tafla and choose to authenticate. Your browser opens on Tafla, you sign in if you aren't, and approve. That's it.

Any other host that speaks MCP over HTTP with OAuth can use the server directly, without this plugin:

```json
{
  "mcpServers": {
    "tafla": { "type": "http", "url": "https://www.tafla.is/api/mcp" }
  }
}
```

The full setup, the tool's rules and an example call are at [tafla.is/agents](https://www.tafla.is/agents).

## What is here

- `.claude-plugin/marketplace.json` lists the plugin for `claude plugin marketplace add`.
- `plugins/tafla/.mcp.json` is the server entry.
- `plugins/tafla/skills/tafla/SKILL.md` tells Claude when to offer a walkthrough and what to send.
- `plugins/tafla/server.json` is the manifest for the MCP registry.

---

[![Tafla, calculations you can understand and trust - Listed on Claude AI Directory](https://www.claudeai.directory/badge/tafla-calculations-you-can-understand-and-trust)](https://www.claudeai.directory/launches/tafla-calculations-you-can-understand-and-trust)
