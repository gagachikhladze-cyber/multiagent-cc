# multiagent-cc

A Claude Code plugin that turns Claude into a **director**. Claude doesn't write the code or content itself — it routes each task to a free or cheap CLI agent (opencode, gemini, mimo, codex), runs it on your **real project**, reviews the result, and reports back. You save Claude tokens; the workers do the labor.

## The idea

```
You → /delegate "task"
        │
        ▼
   Claude = DIRECTOR  ──run in your project dir──►  opencode · gemini · mimo · codex
        │                                                    │ (edit real files)
        └──────────── reviews + reports ◄────────────────────┘
```

Claude handles: classification, routing, review, orchestration.
Workers handle: writing code, drafting content, research, refactors.
Claude only does the work itself if **every** fallback agent fails.

---

## Install

```bash
/plugin marketplace add gagachikhladze-cyber/multiagent-cc
```

Then once:

```bash
/setup
```

`/setup` installs all four agents and writes a small state file so `/delegate` never has to re-check tools on every call.

---

## Usage

```bash
/delegate write unit tests for src/auth.py
/delegate --review refactor the booking flow to remove duplication
/delegate --parallel build the API routes and the React components
/delegate research the 2026 wedding-photography pricing in Kutaisi
```

- **`--review`** — Claude reads the changed files, runs build/test, and (for code) can send it to codex for an adversarial check. Default review is minimal (just `git diff --stat` + the agent's summary) to keep token use low.
- **`--parallel`** — Claude splits the task into subtasks with **non-overlapping file scopes** and runs several agents at once, so they never collide. Default is one-at-a-time.

You can also just ask in plain language ("delegate this and verify it carefully" / "split this across agents").

---

## Agent stack

| Agent | Role | Cost |
|-------|------|------|
| **opencode** | Default code & content worker (many models via `-m`) | Zen Free / Go $10/mo (zero-retention) |
| **gemini** (antigravity) | Research, Google-grounded, big context, fallback | Free 1,000/day |
| **mimo** | Long-horizon autonomous coding (50+ steps) | Free (early access) |
| **codex** | Reviewer / adversarial checker for critical code | ChatGPT Plus $20/mo |

### Fallback chain (code)
```
opencode/mimo-v2.5-free → opencode-go/deepseek-v4-flash
  → opencode-go/mimo-v2.5-pro → gemini → Claude (last resort)
```

### Notes
- **Georgian** works well on OpenCode Go DeepSeek and on Gemini — not only Claude.
- **DeepSeek Zen Free** stores data → use for public/non-sensitive code only. OpenCode Go's DeepSeek is zero-retention.
- opencode must be **≥ 1.17** (older builds have a SQLite `run` bug); `/setup` handles the upgrade.

---

## What's included

```
multiagent-cc/
├── commands/
│   ├── delegate.md   → /delegate [--review] [--parallel] <task>
│   └── setup.md      → /setup
└── skills/
    ├── delegate/     → director routing, execution, review, parallel
    └── setup/        → install all agents + write state.json
```

## What's NOT included

- API keys / credentials — one-time browser logins stay yours (gemini Google auth, opencode `/connect`, codex `login`)
- OpenCode proxy setup (personal)
- MCP server config (ClickUp / Gmail / Telegram etc. — install separately)

## Requirements

- Claude Code (CLI or desktop)
- Node.js 18+
- Optional: ChatGPT Plus (codex), OpenCode Go subscription (zero-retention models)

## License

MIT
