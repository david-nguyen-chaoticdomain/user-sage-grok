---
name: usersage-results
description: "Read back User Sage study results. Use when the user asks what a study found, wants findings across one or many studies, needs specific responses or quotes, or wants to inspect a study's setup."
---

# User Sage: Study Results

## Tools

- **`list_studies`**: lists every study in the workspace: id, name, method, status, live response counts. Call this first to find a study by name or topic, or to get every study id before synthesizing findings across studies (for example, "what have we learned about onboarding across all our research").
- **`get_study_findings`**: what a study found, summarized: takeaways, per-question totals, task outcomes, strongest card groupings. Use this first for any question about results. It includes `resultsUrl`, the full visual results dashboard.
- **`get_study_responses`**: individual responses, one page at a time (up to 20 per call), for when the summary is not enough: specific quotes or particular people. `source` picks AI Panel or real participants. Fetch only the pages you actually need.
- **`get_study`**: the study's full configuration and every raw result. Large. Use only when you need the exact setup (its questions, tasks, or steps).

## Plans

The tools in this skill need a Pro workspace (the 7-day Pro trial counts). On a Free workspace a call is not an error: User Sage replies with `upgradeRequired`, a plain explanation, the matching page in the app (`dashboardUrl`) and the billing link (`upgradeUrl`). Relay that plainly: say the feature needs Pro, offer the billing link, and point to the same thing in the app, where it works on Free. Do not call these tools just to find out, and never retry a refused call. A researcher on Free can still plan a study and list their Briefs from chat: see the `usersage-plan` skill.

## Auth

The server is `https://mcp.usersage.com/mcp`, configured by this plugin with an `Authorization: Bearer ${USERSAGE_API_KEY}` header. The key is the user's own User Sage API key, created in the User Sage dashboard under Workspace settings, Integrations, MCP Connector. Any plan can connect. On an auth error (401), tell the user to check that `USERSAGE_API_KEY` is set in the shell Grok was started from and that the key has not been revoked.

## Rules

- **Keep AI Panel results and real-participant results separate** when you relay findings, and always say which each comes from. They are different evidence.
- **Start from the summary.** Call `get_study_findings` before `get_study_responses`; only page through individual responses when the user needs quotes or specific people.
- **Do not re-run or re-recruit from here.** If the researcher wants another run of the same study, that is `duplicate_study` plus a recruitment path; see the `usersage-recruit` skill.
- Studies still collecting from a real panel are fine to read: findings include live Your Panel responses and any generated takeaway so far. Say the study is still collecting when that is the case.
- **If a tool named here is missing,** your client cached an older tool list: reconnect the User Sage MCP server or start a new Grok session.
