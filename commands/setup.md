---
description: One-time install + health-check for the delegation stack (opencode, gemini, mimo, codex). Writes a state file so /delegate never re-checks. Run again to repair.
argument-hint: ""
---

Invoke the `setup` skill.

Install or upgrade all four agents (opencode ≥1.17, gemini, mimo, codex), run the smoke tests, write `~/.claude/multiagent-cc/state.json` with real readiness values, and report a clear status summary. List any one-time browser logins I still need to do manually (gemini Google auth, opencode /connect, mimo MiMo Auto, codex login).
