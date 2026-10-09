# User Sage for Grok

This repo holds two things. The **bot** is how people get User Sage inside Grok today. The **plugin** is built but on hold, because xAI has no marketplace open to smaller brands yet.

## The User Sage bot (Grok Bot)

The User Sage bot for xAI's Grok Bot desktop app. Marketplace listings are not open
yet, so the bot is how people get User Sage inside Grok today: they add it from a share
link, use it with no account, and connect their User Sage workspace when they want to
build and run real studies.

Share link: https://x.ai/bot/KmP4fr1ttidksZkt3i_fd (the **Add to Grok Bot** button
installs a copy).

### What the bot does

An on-demand design critic and UX research assistant.

- **Without an account (advisory mode):** design critique and scoring, attention
  predictions, copy revision, inclusive design checks, method picks, study plans, survey
  checks and findings analysis. Anything simulated is labeled as simulated.
- **With the User Sage connector:** plan studies, and on Pro create them, run an AI
  Panel, recruit, read results, and work with Projects, personas and panels.

### The connector (what the bot can call)

The MCP server is `https://mcp.usersage.com/mcp`. Auth is an org-scoped API key sent as
a Bearer token (`usg_live_...`), not OAuth. Any plan can create a key and connect. What
the key can do depends on the plan:

- **Free:** `plan_study`, `list_briefs`.
- **Pro (and the 7-day trial):** all 20 tools. The rest are `create_study`,
  `duplicate_study`, `run_ai_panel`, `start_your_panel_recruitment`,
  `launch_genpop_recruitment`, `get_recruitment_options`, `list_studies`, `get_study`,
  `get_study_findings`, `get_study_responses`, `list_projects`, `get_project`, `list_personas`,
  `get_persona`, `create_persona`, `list_panels`, `get_panel`, `create_panel`.

A Free key calling a Pro tool gets a normal reply (not an error) saying the tool needs
Pro, with a billing link and a dashboard link. The bot relays it as written.

Where it lives: server in `user-sage-backend/apps/mcp`, tier rules in
`user-sage-shared/docs/HELP-ME-CREATE-STUDY.md` (decision 20), the AI behind
`plan_study` and simulated people in `user-sage-shared/docs/SAGE-ENGINE.md`.

### What goes where in the bot

In the bot's Details, the profile (name, "Last updated" date and the short description) sits at
the top. Under it, the **Context** row opens Instructions, Memories and Skills. The Instructions
field is separate from the description. The Grok Bot app has three places to put things:

- **Instructions** are the always-on rules: how the bot behaves, its tone and product rules.
- **Skills** are reusable recipes the bot runs for a kind of ask, like critique, plan a study
  or survey checkup.
- **Memories** are facts it remembers, such as a name, a preference or a past decision.

All three ship with the share when you Publish.

How we use them:

- The profile line is the short public blurb on the share page. Instructions hold a few
  short always-on rules (evidence first, label what is simulated, trust the connector,
  confirm before spending, never ask for a key in chat). Keep them short.
- The working rules live in skills, so each one loads only when it is needed.
- Memories hold the few facts that must always be in play: who made the bot, its version,
  support, naming, how to onboard the connector. Every new install inherits every memory, so they
  never hold anything specific to one person.

### Updating the bot

The bot's profile, skills and memories are kept in
[`USER-SAGE-GROK-BOT-INVENTORY.md`](./USER-SAGE-GROK-BOT-INVENTORY.md). Edit that file, paste it
into the bot to apply it, then update the template and publish by hand. The steps and the
checks to run afterwards are at the top of the file.

### Things to know when changing the bot

- The profile text is the public blurb on the share page. The working rules live in the
  bot's skills and memories.
- Installs are copies. Editing the bot does not update people who already added it:
  they remove it and add it again from the share link, then reconnect the connector.
- The connector belongs to each person's own copy, so the template never carries it.
- Server changes (new tools, new replies) reach connected users without a reinstall, but
  a client may keep its old tool list until the connector is reconnected.
- Changes to the shared bot go live only when the template is updated and published by
  hand in the Grok Bot app.

## The Grok plugin (on hold)

[User Sage](https://usersage.com) is AI-assisted user testing. This plugin connects Grok to your User Sage workspace through the hosted MCP server, so your agent can plan UX research studies, draft them, recruit participants, and read back findings without leaving the terminal.

### What it does

Five skills, one MCP server (`https://mcp.usersage.com/mcp`, 20 tools):

| Skill | What it does |
|---|---|
| `usersage-plan` | Plan a study from a research question: `plan_study` writes a Study Brief (a recommended method, a drafted study, who to ask) and returns a link to open it in User Sage; `list_briefs` finds Briefs made earlier. Works on every plan. |
| `usersage-draft` | Draft studies with `create_study`: surveys, tree tests, card sorts, five-second tests, preference tests, first-click tests, prototype (click task flow) tests. Variation copies with `duplicate_study`. |
| `usersage-recruit` | Launch recruitment: Your Panel (own audience, free), AI Panel (synthetic personas, spends AI credits), GenPop Panel (real screened participants, spends wallet funds). |
| `usersage-results` | Read back findings (`get_study_findings`), individual responses (`get_study_responses`), and study setups. |
| `usersage-workspace` | Projects (`list_projects`, `get_project`), personas (`list_personas`, `get_persona`, `create_persona`) and AI panels of simulated people (`list_panels`, `get_panel`, `create_panel`). |

### What each plan can do

Any plan can connect. **Free:** plan a study with `plan_study` and list your Briefs with `list_briefs`. Each gives a link to open in User Sage, where you create the study from the Brief for free and do everything else. **Pro (and the 7-day Pro trial):** everything else from your agent: create and duplicate studies, run an AI Panel, start recruiting, read studies and results, and work with Projects, personas and AI panels. Ask for something that needs Pro on a Free workspace and User Sage replies with a plain explanation and a link to billing, never an error, so your agent can tell you and suggest it.

### Installation

1. Install Grok Build and sign in (`grok login`).
2. Get a User Sage API key: in the User Sage dashboard, go to Workspace settings, Integrations, MCP Connector, name a key and generate it. Keys start with `usg_live_`.
3. Export it in your shell before starting Grok:
   ```bash
   export USERSAGE_API_KEY="usg_live_..."
   ```
4. In Grok, open `/marketplace`, find **user-sage**, press `i` to install.
5. Ask Grok to plan a study, draft one, recruit a panel, or summarize findings.

If a tool this plugin names is missing after an update, your client cached an older tool list: reconnect the User Sage MCP server or start a new Grok session.

If your Grok client does not expand `${USERSAGE_API_KEY}` from the plugin's bundled `.mcp.json`, register the server manually in your own Grok MCP config with the same URL and an `Authorization: Bearer` header carrying your key (the same pattern TinyFish documents for API-key auth).

### Authentication

User Sage's MCP server has no OAuth flow; it authenticates per-user Bearer API keys. The plugin's `.mcp.json` declares the header as `Authorization: Bearer ${USERSAGE_API_KEY}` and reads nothing else: no `.env` files, no password managers, no other endpoints. Each installing user supplies their own key, so usage is billed to their own workspace.

Requirements worth knowing up front:

- Any plan can connect (see "What each plan can do"). On a 401, check the key.
- The connector **cannot edit existing studies**. The skills steer the agent to duplicate a study for variations instead.

### Network endpoints and credentials (for review)

- One MCP connection: `https://mcp.usersage.com/mcp` (Streamable HTTP).
- One credential: the `USERSAGE_API_KEY` environment variable, sent as a Bearer token to that endpoint only.
- No scripts, hooks, binaries, or install steps ship in this plugin. Contents are Markdown and JSON only.

### Submitting to the marketplace (maintainer steps)

Status: on hold. xAI has no marketplace open to smaller brands yet. The catalog PR ([xai-org/plugin-marketplace#1037](https://github.com/xai-org/plugin-marketplace/pull/1037)) is open and unreviewed, and it pins the old owner URL and an old commit, so refresh both before it moves.

This repo is the plugin source. To list it in the official catalog:

1. Keep this public repo under the official org (`user-sage/user-sage-grok`). Do not publish from a personal throwaway account; xAI flags branded plugins sourced from personal accounts as possible impersonation.
2. Fork [`xai-org/plugin-marketplace`](https://github.com/xai-org/plugin-marketplace) and branch from `main`.
3. Add one entry to `.grok-plugin/marketplace.json`:
   ```json
   {
     "name": "user-sage",
     "description": "AI-assisted user testing: plan and draft UX research studies, recruit Your/AI/GenPop panels, and read back findings.",
     "category": "research",
     "source": {
       "source": "url",
       "url": "https://github.com/user-sage/user-sage-grok.git",
       "sha": "<full-40-char-commit-sha>"
     },
     "homepage": "https://usersage.com",
     "keywords": ["user sage", "usersage", "user testing", "usability testing", "ux research"],
     "domains": ["usersage.com", "mcp.usersage.com"]
   }
   ```
   Get the SHA with `git ls-remote https://github.com/user-sage/user-sage-grok.git HEAD`. Pin a real commit, never a branch or tag.
4. Regenerate the component index (never hand-edit it) and validate, exactly as CI does:
   ```bash
   python3 scripts/generate-plugin-index.py
   python3 scripts/validate-catalog.py
   python3 scripts/generate-plugin-index.py --check
   ```
5. Open the PR, fill in the template, and wait for CI plus code-owner review. To ship an update later, bump the pinned `sha` and regenerate the index; do not open a parallel duplicate entry.

Keywords and domains are brand-scoped on purpose: they power Grok's plugin suggestion CTA, and generic terms get pushed back in review.

### Support

help@usersage.com
