---
name: usersage-recruit
description: "Recruit participants for a User Sage study. Use when the user wants to launch a study: Your Panel (own audience, free), AI Panel (synthetic personas, spends AI credits), or GenPop Panel (real screened participants, spends wallet funds)."
---

# User Sage: Recruit Participants

Three paths, three different costs. Call them by these names: Your Panel, AI Panel, GenPop Panel.

## Tools

- **`get_recruitment_options`** — call this BEFORE proposing any recruitment path. It reports which paths can start on this study right now and why an unavailable one is unavailable; relay that as-is and never call that tool anyway. It also covers the AI credit balance and cost, the available AI panels and per-run persona limits, and the GenPop wallet balance plus its exact cost formula, so you can work out affordable sample sizes yourself rather than guessing.
- **`start_your_panel_recruitment`** — real, shareable link recruiting from the org's own audience. Free. `open` mode: anyone with the link can respond. `closed` mode: only the emails in `inviteEmails`, and this only builds an allowlist, it does NOT send anyone an email; make sure the researcher knows they still need to share the link themselves. This tool never distributes the link for them.
- **`run_ai_panel`** — synthetic personas take the study; results come back in minutes. Costs AI credits (charged once when the run starts). Asynchronous: hand the researcher `resultsUrl` to watch it live, then check `get_study_findings` later. A study runs through AI Panel once; never suggest re-running the same study. A repeat needs a copy via `duplicate_study`.
- **`launch_genpop_recruitment`** — real, screened human participants. Spends real GenPop wallet funds the moment it succeeds, and calling it again does not undo that. Work out the cost against the wallet balance and confirm the numbers with the researcher before calling.

## Auth

The server is `https://mcp.usersage.com/mcp`, configured by this plugin with an `Authorization: Bearer ${USERSAGE_API_KEY}` header. The key is the user's own User Sage API key, created in the User Sage dashboard under Workspace settings, Integrations, MCP Connector. MCP access requires a Pro workspace. On an auth error, tell the user to check that `USERSAGE_API_KEY` is set in the shell Grok was started from, and that their workspace is on Pro.

## Rules

- **Always call `get_recruitment_options` first.** Never propose or launch a path without it.
- **Spending money or credits needs explicit confirmation.** State the cost (AI credits for AI Panel; the computed wallet cost for GenPop) and get a yes before calling `run_ai_panel` or `launch_genpop_recruitment`.
- **If a balance is too low, say so plainly** and point at the top-up page from the tool response (`buyMoreUrl`) instead of attempting the call anyway.
- **Never poll a running AI Panel run by re-calling `run_ai_panel`.** Check `get_study_findings`; status stays `running` until `complete` or `failed` (a failed run refunds credits automatically).
- After any recruitment starts, point the researcher at `resultsUrl` and the `usersage-results` skill for reading what comes back.
