---
name: setup
description: One-time install and health-check for the delegation stack — installs opencode, gemini, mimo, and codex, then writes a state file so /delegate never has to re-check. Run once, or again to repair.
---

# Setup — Install All Agents, Write State

Goal: get all four workers ready, then record readiness in `~/.claude/multiagent-cc/state.json` so `/delegate` can skip checks on every call.

Run the steps below with the Bash tool.

## Step 1 — Check what exists (parallel)

```bash
opencode --version 2>&1 | tail -1
gemini --version 2>&1 | tail -1
mimo --version 2>&1 | tail -1
codex --version 2>&1 | tail -1
node --version 2>&1
```

## Step 2 — Install / upgrade

Install anything missing. **opencode must be ≥ 1.17** (older versions have a `session_message.seq` SQLite bug that breaks `run`).

```bash
# opencode (install or upgrade — always safe to run)
npm install -g opencode-ai@latest

# gemini CLI (antigravity)
npm install -g @google/gemini-cli@latest

# mimo
npm install -g @mimo-ai/cli || curl -fsSL https://mimo.xiaomi.com/install | bash

# codex (OpenAI)
npm install -g @openai/codex@latest
```

## Step 3 — Verify auth & execution

Some logins need a browser and can't be automated — detect and instruct.

```bash
# opencode providers (need OpenCode Go and/or Zen)
opencode providers list 2>&1
# Expect "OpenCode Go" / "OpenCode Zen". If 0 credentials → user runs `opencode` → /connect once.

# gemini smoke test
gemini -p "say ok" --skip-trust 2>&1 | tail -3
# If auth error → user runs `gemini` once → Google login.

# opencode execution smoke test (proves the run bug is gone)
opencode run "say ok" -m opencode-go/deepseek-v4-flash --dangerously-skip-permissions 2>&1 | tail -3

# codex auth
codex exec "say ok" 2>&1 | tail -3
# If not logged in → user runs `codex login` (ChatGPT Plus).

# mimo
mimo --version 2>&1 | tail -1
# If installed but never configured → user runs `mimo` once → select "MiMo Auto".
```

## Step 4 — Write state file

Create the directory and write readiness based on what actually passed above. Set each `ready` to true only if its smoke test returned a real response (not an error).

```bash
mkdir -p ~/.claude/multiagent-cc
cat > ~/.claude/multiagent-cc/state.json <<'JSON'
{
  "verifiedAt": "REPLACE_WITH_DATE",
  "tools": {
    "opencode": { "ready": true,  "version": "REPLACE" },
    "gemini":   { "ready": true },
    "mimo":     { "ready": false },
    "codex":    { "ready": false }
  }
}
JSON
```

Before writing, substitute real values: set each `ready` from the smoke-test results, fill `version` from Step 1, and replace `REPLACE_WITH_DATE` with the output of `date -u +%Y-%m-%dT%H:%M:%SZ`. Re-emit the heredoc with the correct JSON — don't leave placeholders.

## Step 5 — Report

Print a clear summary, e.g.:

```
✓ opencode v1.17.7 — ready (Go + Zen connected)
✓ gemini v0.42.0 — ready (Google auth OK)
✓ mimo v0.1.1 — installed (run `mimo` once to pick MiMo Auto)
⚠ codex — installed, needs `codex login` (ChatGPT Plus)

state.json written. Ready: /delegate "your task"
```

List any manual one-time browser steps the user still needs to do. Those are the only things setup can't finish automatically:

- **gemini** — `gemini` → Google OAuth
- **opencode Go/Zen** — `opencode` → `/connect`
- **mimo** — `mimo` → select "MiMo Auto"
- **codex** — `codex login` (ChatGPT Plus/Pro)
