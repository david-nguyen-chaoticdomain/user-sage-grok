# User Sage plugin for Grok

[User Sage](https://usersage.com) is AI-assisted user testing. This plugin connects Grok to your User Sage workspace through the hosted MCP server, so your agent can draft UX research studies, recruit participants, and read back findings without leaving the terminal.

## What it does

Three skills, one MCP server (`https://mcp.usersage.com/mcp`, 10 tools):

| Skill | What it does |
|---|---|
| `usersage-draft` | Draft studies with `create_study`: surveys, tree tests, card sorts, five-second tests, preference tests, first-click tests, prototype (click task flow) tests. Variation copies with `duplicate_study`. |
| `usersage-recruit` | Launch recruitment: Your Panel (own audience, free), AI Panel (synthetic personas, spends AI credits), GenPop Panel (real screened participants, spends wallet funds). |
| `usersage-results` | Read back findings (`get_study_findings`), individual responses (`get_study_responses`), and study setups. |

## Installation

1. Install Grok Build and sign in (`grok login`).
2. Get a User Sage API key: in the User Sage dashboard, go to Workspace settings, Integrations, MCP Connector, name a key and generate it. Keys start with `usg_live_`.
3. Export it in your shell before starting Grok:
   ```bash
   export USERSAGE_API_KEY="usg_live_..."
   ```
4. In Grok, open `/marketplace`, find **user-sage**, press `i` to install.
5. Ask Grok to draft a study, recruit a panel, or summarize findings.

If your Grok client does not expand `${USERSAGE_API_KEY}` from the plugin's bundled `.mcp.json`, register the server manually in your own Grok MCP config with the same URL and an `Authorization: Bearer` header carrying your key (the same pattern TinyFish documents for API-key auth).

## Authentication

User Sage's MCP server has no OAuth flow; it authenticates per-user Bearer API keys. The plugin's `.mcp.json` declares the header as `Authorization: Bearer ${USERSAGE_API_KEY}` and reads nothing else: no `.env` files, no password managers, no other endpoints. Each installing user supplies their own key, so usage is billed to their own workspace.

Requirements worth knowing up front:

- MCP access is **Pro-gated**: free workspaces cannot use the connector. On a 401/403, check the key and the workspace plan.
- The connector **cannot edit existing studies**. The skills steer the agent to duplicate a study for variations instead.

## Network endpoints and credentials (for review)

- One MCP connection: `https://mcp.usersage.com/mcp` (Streamable HTTP).
- One credential: the `USERSAGE_API_KEY` environment variable, sent as a Bearer token to that endpoint only.
- No scripts, hooks, binaries, or install steps ship in this plugin. Contents are Markdown and JSON only.

## Submitting to the marketplace (maintainer steps)

This repo is the plugin source. To list it in the official catalog:

1. Create a public repo under the official org (for example `david-nguyen-chaoticdomain/user-sage-grok`) and push these files. Do not publish from a personal throwaway account; xAI flags branded plugins sourced from personal accounts as possible impersonation.
2. Fork [`xai-org/plugin-marketplace`](https://github.com/xai-org/plugin-marketplace) and branch from `main`.
3. Add one entry to `.grok-plugin/marketplace.json`:
   ```json
   {
     "name": "user-sage",
     "description": "AI-assisted user testing: draft UX research studies, recruit Your/AI/GenPop panels, and read back findings.",
     "category": "research",
     "source": {
       "source": "url",
       "url": "https://github.com/david-nguyen-chaoticdomain/user-sage-grok.git",
       "sha": "<full-40-char-commit-sha>"
     },
     "homepage": "https://usersage.com",
     "keywords": ["user sage", "usersage", "user testing", "usability testing", "ux research"],
     "domains": ["usersage.com", "mcp.usersage.com"]
   }
   ```
   Get the SHA with `git ls-remote https://github.com/david-nguyen-chaoticdomain/user-sage-grok.git HEAD`. Pin a real commit, never a branch or tag.
4. Regenerate the component index (never hand-edit it) and validate, exactly as CI does:
   ```bash
   python3 scripts/generate-plugin-index.py
   python3 scripts/validate-catalog.py
   python3 scripts/generate-plugin-index.py --check
   ```
5. Open the PR, fill in the template, and wait for CI plus code-owner review. To ship an update later, bump the pinned `sha` and regenerate the index; do not open a parallel duplicate entry.

Keywords and domains are brand-scoped on purpose: they power Grok's plugin suggestion CTA, and generic terms get pushed back in review.

## Support

help@usersage.com
