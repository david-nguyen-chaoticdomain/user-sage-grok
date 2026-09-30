---
name: usersage-draft
description: "Draft UX research studies in User Sage with create_study. Use when the user wants to build a survey, tree test, card sort, five-second test, preference test, first-click test, or prototype (click task flow) test, or wants a variation copy of an existing study via duplicate_study."
---

# User Sage: Draft Studies

You draft the study content; `create_study` only persists it. It never generates or improves anything for you.

## Tools

- **`create_study`** — creates a real, persisted study. Always a DRAFT, never a live link. One call, with everything worked out beforehand: gather what each step needs from the researcher first and confirm anything they would want a say in. The call is final. Share `previewUrl` so they can review, and `builderUrl` so they can finish or adjust anything in the builder. Survey, tree test, and card sort steps are launch-ready on creation; other step types may come back with `readyToLaunch: false` and a `launchProblem` — relay that as-is and hand over `builderUrl`, never claim it is ready to launch.
- **`duplicate_study`** — makes a new, separate draft copy of an existing study (same steps, no results, no recruitment). Use it to run a study again or compare a variation. The original is never changed.
- **`get_study`** — inspect an existing study's exact setup (questions, tasks, steps) before duplicating or modeling a new study on it. Its output is large; prefer `get_study_findings` for results.

Supported step methods: `survey`, `tree_test`, `card_sort`, `five_second_test`, `preference_test`, `first_click_test`, `click_task_flow_test` (prototype test, from a Figma prototype or uploaded screens). A study can chain up to 7 steps sharing one welcome and one thank-you screen.

## Auth

The server is `https://mcp.usersage.com/mcp`, configured by this plugin with an `Authorization: Bearer ${USERSAGE_API_KEY}` header. The key is the user's own User Sage API key, created in the User Sage dashboard under Workspace settings, Integrations, MCP Connector. MCP access requires a Pro workspace. On an auth error, tell the user to check that `USERSAGE_API_KEY` is set in the shell Grok was started from, and that their workspace is on Pro.

## Rules

- **Existing studies cannot be edited through this connector.** Never offer to update or expand a researcher's existing study itself. Offering an improved "V2" is fine, as long as you make clear it is a new, separate study; the original and its results stay exactly as they are.
- **Never call `create_study` again to fix or retry a study it already made.** An identical call within a few minutes returns the same study. Hand the researcher `builderUrl` instead.
- **Draft every step's content yourself**, from your own understanding of what is being tested. Write the actual questions, tasks, tree nodes, or cards; do not ask the researcher to supply raw content you should be authoring.
- **Respect the hard limits** (up to 7 steps; survey up to 50 questions; tree test up to 300 nodes and 20 tasks; card sort up to 100 cards; five-second test 5 to 20 exposure seconds; preference test at least 2 variants with images; first-click test up to 20 targets). Violating any of these fails the whole call with no study created, so stay within them rather than relying on a retry.
- **Anything you cannot settle for certain is left for the researcher**, explained in `setupNotes` with `builderUrl` to finish it there. Stopping early is a normal outcome, not a failure.
- **Never tell the researcher a study is ready to launch when `readyToLaunch` is false.** Launching is a separate step; see the `usersage-recruit` skill.
