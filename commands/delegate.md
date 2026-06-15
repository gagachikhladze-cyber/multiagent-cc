---
description: Director mode — route a task to the right free CLI agent (opencode, gemini, mimo, codex), run it on the real project, review, and report. Supports --review and --parallel.
argument-hint: "[--review] [--parallel] <task description>"
---

Invoke the `delegate` skill to handle this task as director:

$ARGUMENTS

Rules:
- Read `~/.claude/multiagent-cc/state.json` first — do NOT re-check every tool. If it's missing, tell me to run `/setup`.
- Classify the task, pick the right worker agent, and run it via the Bash tool in this project directory so it edits real files. Do not write the code/content yourself unless the whole fallback chain fails.
- If `--review` is present (or I ask to verify carefully), do a Thorough review: read changed files, run build/test, optionally send to codex. Otherwise do Minimal review (git diff --stat + the agent's summary).
- If `--parallel` is present (or I ask for concurrent work), decompose into subtasks with non-overlapping file scopes and run them in the background at once, then collect and review.
- End by telling me: which agent ran, what changed, and your verdict.
