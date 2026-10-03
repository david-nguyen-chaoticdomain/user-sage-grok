---
name: usersage-workspace
description: "Work with a User Sage workspace's Projects, personas and AI panels. Use when the user wants to see their Projects, list or create a persona, see or create an AI panel of simulated people, or pick a panel for an AI Panel run. Pro workspaces."
---

# User Sage: Projects, Personas and AI Panels

A **persona** is a reusable archetype of how a kind of user behaves. A **panel** is a named group of **simulated people**, each a made-up individual generated from a persona, that an AI Panel run uses. Simulated people are a hypothesis tool, never real people.

## Plans

These tools need a Pro workspace (the 7-day Pro trial counts). On Free a call is not an error: User Sage replies with `upgradeRequired`, a plain explanation, the matching page in the app (`dashboardUrl`) and the billing link (`upgradeUrl`). Relay that plainly, offer the billing link, point to the same page in the app, and never retry. Do not call these tools just to find out.

## Tools

- **`list_projects`**: the workspace's Projects (groups of related studies and Briefs): name, status, how many studies and Briefs, and `projectUrl`. A Project id is `project_id` on `plan_study` and `list_briefs`.
- **`list_personas`** and **`get_persona`**: the personas available (the workspace's own and the built-in ones), then one in full: goals, pain points, behaviors, what to design with and against, and five trait scores (0 to 100: patience, risk tolerance, tech fluency, trust, attention to detail).
- **`create_persona`**: saves a persona you drafted from what the researcher told you; it never generates or improves anything. Free to use, no credits.
- **`list_panels`** and **`get_panel`**: the AI panels available, then one with its simulated people (name, age, job, city, pace, trust, traits). Full profiles are in the app.
- **`create_panel`**: generates simulated people from one persona (1 to 20, six is typical) and saves them as a new panel. Free, no credits; credits are spent when a study is run on it. Next step: `run_ai_panel` with the new `panel_id` (see the `usersage-recruit` skill).

## Rules

- **A persona is behavioral, not demographic.** Never put age, gender, ethnicity, income or any other demographic in one, and keep it plausible, not stereotyped. Give the five trait scores when you can: they decide how the simulated people behave (low patience gives up sooner, low trust doubts claims).
- **Confirm before saving.** Show the researcher the persona (or the panel name, persona and number of people) and get a yes before `create_persona` or `create_panel`.
- **Existing personas and panels cannot be edited or deleted through this connector.** Send the researcher to the app (the `personasUrl` and `panelsUrl` in the replies) to change them.
- **Keep the label.** Say simulated people are made up and a hypothesis, not real people; AI Panel results are directional, and real people confirm (Your Panel or GenPop Panel).
- **If a tool named here is missing,** your client cached an older tool list: reconnect the User Sage MCP server or start a new Grok session.
