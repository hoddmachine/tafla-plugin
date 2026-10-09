---
name: tafla
description: Show the working behind a calculation as a Tafla walkthrough, by link. Use when the user asks to see, explain, share, visualise or double-check a calculation, or right after you have worked out a number from several givens that a reader would want to see laid out, such as a payment, a projection, a comparison or a yes-or-no verdict on figures.
---

# Tafla

Tafla turns a calculation into a walkthrough: the givens, each step with its formula, the result circled, and captions or a voice that talk the reader through it. The calculation is live, so the reader can drag an input and watch every step recalculate. The link works for anyone and needs no account to open.

Call the `explain_calculation` tool on the `tafla` MCP server. It builds the calculation from the formulas you send, computes every value with a spreadsheet engine, writes the walkthrough, stores it on the user's Tafla account and answers with its link and the values.

## If the server is not available

The tool needs the `tafla` MCP server, signed in to the user's Tafla account. If the server is missing, or answers 401, do not work around it:

1. If the server is missing: the plugin carries it, so reinstall the plugin with `claude plugin install tafla@tafla`, or enable its server under `/mcp`. The server loads at session start, so then either start a new session or make the call from a headless one (below).
2. If it answers 401, sign the user in yourself. Run `claude mcp login plugin:tafla:tafla`. It opens the browser on Tafla, the user approves once, and the command finishes on its own. Tell the user the browser is open and what to click.
   - If it refuses because stdin is not a terminal, run it inside one: `script -q /dev/null claude mcp login plugin:tafla:tafla < /dev/null` (macOS and Linux).
   - If `claude mcp login` does not exist, Claude Code is older than 2.1. Say so, suggest `claude update`, and meanwhile the user can type `/mcp`, pick tafla and choose to authenticate.

Claude Code may ask the user to approve the install or the sign-in before it runs. That is its permission check, not a failure: say what it is asking for and carry on once they say yes.

The details are at https://www.tafla.is/agents.md (the same page in the browser is https://www.tafla.is/agents).

## If the tool is not loaded in this session

MCP servers load when a session starts. If the server was installed or signed in during this session, the tool is not here yet, but a fresh headless session has it. Make the call from one and relay what it returns, instead of telling the user to restart:

```sh
claude -p --allowedTools mcp__plugin_tafla_tafla__explain_calculation \
  "Call explain_calculation on the tafla MCP server with: <the title, inputs and formulas, spelled out>. Reply with the link and the values it returned, nothing else."
```

Run it with `env -u CLAUDECODE` in front if it refuses to start inside Claude Code. The tool's name follows the server: `mcp__plugin_tafla_tafla__explain_calculation` from the plugin, `mcp__tafla__explain_calculation` when the server was added by hand with `claude mcp add`.

## When to offer it

- The user asks to see, explain, share or check a calculation.
- You have just worked out a number from several givens (a loan payment, a break-even, a tax figure, a conversion chain). Offer the walkthrough in one line, or make it when the user has asked for visuals before.
- Not for a single multiplication, a lookup, or a number with no working behind it.

## What to send

- `title`: the question the calculation answers, as the reader would ask it, in the reader's language.
- `inputs`: every given, with a short `label`, a `unit` where there is one, and a sensible `min`, `max` and `step` so the slider has a real-world range. A choice between named alternatives takes `options` instead of a range.
- `formulas`: every derived quantity, in build order, as an Excel formula over earlier names. A formula may use any input and any formula before it.
- A `description` on each step: one plain sentence saying what it is. Tafla writes the walkthrough from the labels and descriptions, so they carry the explanation.

## Rules

- Never put a number you computed into an input. Inputs are givens; everything else is a formula.
- Show the mechanism. `PMT`, `FV`, `PV`, `NPER` and their kin are refused. Build the closed form as named steps: `growth_factor = (1+rate/100)^n`, then `payment = amount*(rate/100)*growth_factor/(growth_factor-1)`.
- Rates are the number a person says: 6 with unit `%`, divided by 100 inside the formulas that use it.
- Four to ten cards explain better than twenty. Skip restatements of the same quantity.
- Names are English snake_case. The title, labels and descriptions are in the reader's language.

## After the call

- Give the user the link.
- Quote the engine's values, not yours. If they differ from what you worked out, say so and work out why.
- If a formula errored, nothing was created. Fix the formula and call again.
