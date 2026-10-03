---
name: usersage-plan
description: "Plan a UX research study in User Sage before building it. Use when the user has a research question but not yet a method, wants a recommended method and a drafted study, or wants to find a Study Brief they made earlier. Works on every plan, Free included."
---

# User Sage: Plan a Study

`plan_study` turns a research question into a **Study Brief**: a recommended method with why, a runner-up, a drafted study, who to ask, how many people, a suggested way to recruit, and a SIMULATED read with a verdict on whether real people are needed. It saves the Brief in the researcher's workspace and returns `briefUrl`, a link to open it in the User Sage app. It never creates a study, contacts anyone or spends recruitment money.

## Tools

- **`plan_study`**: costs 4 credits, charged only when a plan is produced. Confirm with the researcher before calling. Call it once per question: repeating the exact same call within a few minutes returns the same plan without charging again. Context improves the plan: `decision`, `users`, `testing` (the product or screen and its stage), `out_of_scope`, `timeline`, `success_metrics`, `stimulus_url`, and `project_id` (Pro workspaces find one with `list_projects`).
- **`list_briefs`**: the workspace's Briefs, newest first, each with its name, goal, method, status, Project and a `briefUrl`. Use it to find a Brief the researcher made before, to check what they already planned, or to hand over a link. It never shows a Brief's contents: the link is the way in.

## Flow

1. Ask what you need first: the decision this research should inform, who the participants are, and what they already have (a Figma link, screenshots, a site map).
2. Say `plan_study` costs 4 credits and, once they agree, call it once.
3. Show the recommended method and why, the runner-up, the drafted study, the audience, the suggested number of participants, the SIMULATED read (label it SIMULATED; it is not evidence) and the verdict, and always the `briefUrl`.
4. Let them change anything. Relay `clarifyingQuestions` and ask where participants should come from.
5. The next step depends on the plan:
   - **Free:** give them `briefUrl`. They open the Brief in User Sage and choose Create study draft, which is free, and do everything else there. Do not offer to create the study from chat.
   - **Pro (and the 7-day trial):** after they approve, pass the reply's `createStudyInput` to `create_study` (see the `usersage-draft` skill).

## What stays the researcher's

The plan keeps the researcher's own words as given. Everything else is a proposal, and the guesses that matter are listed in `assumptions`. Relay the assumptions, and do not present a proposed decision, success target or audience as something the researcher said. The simulated read talks about what people generally expect for the topic, never about the contents of a design the planner has not seen.

## When something needs Pro

If a Pro feature would help (creating the study from chat, running an AI Panel, reading results, Projects, personas, panels), you may and should suggest it. Say plainly that it needs a Pro workspace (the 7-day Pro trial counts) and offer the billing link from the tool's reply. Do not call a Pro tool just to find out, and never retry a refused call: a Free workspace gets a normal reply (`upgradeRequired`, a plain explanation, `dashboardUrl` and `upgradeUrl`), not an error.

## Rules

- **Plan first** when the researcher has a question but no method. Only skip it when they already know exactly what to build.
- **One `plan_study` per question.** To explore a different question, plan again and confirm the 4 credits again.
- **Simulated is not evidence.** Keep the SIMULATED label with any simulated read.
- **If a tool named here is missing,** your client cached an older tool list: reconnect the User Sage MCP server or start a new Grok session.
