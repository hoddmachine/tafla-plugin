---
name: tafla
description: Show the working behind a calculation as a Tafla walkthrough, by link. Use when the user asks to see, explain, share, visualise or double-check a calculation, or right after you have worked out a number from several givens that a reader would want to see laid out, such as a payment, a projection, a comparison or a yes-or-no verdict on figures.
---

# Tafla

Tafla turns a calculation into a walkthrough: the givens, each step with its formula, the result circled, and captions or a voice that talk the reader through it. The calculation is live, so the reader can drag an input and watch every step recalculate. The link works for anyone and needs no account to open.

Call the `explain_calculation` tool on the `tafla` MCP server. It builds the calculation from the formulas you send, computes every value with a spreadsheet engine, writes the walkthrough, stores it on the user's Tafla account and answers with its link and the values.

## If the server is not available

The tool needs the `tafla` MCP server, signed in to the user's Tafla account. If the server is missing, or answers 401, do not work around it. Tell the user:

1. If the server is missing: the plugin carries it, so reinstall the plugin with `claude plugin install tafla@tafla`, or enable its server under `/mcp`, then start a new session.
2. To sign in: type `/mcp`, pick tafla and choose to authenticate. The browser opens on Tafla to approve, once.

The details are at https://www.tafla.is/agents.

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
