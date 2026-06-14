# multiagent-cc

Claude Code plugin that routes tasks to the right free or cheap AI agent — so Claude Pro stays as **director**, not worker.

## The idea

Claude Pro tokens are expensive. Most tasks (routine code, drafts, research) don't need Claude Pro. This plugin adds a `/delegate` command that:

1. Classifies your task (Code / Content / Research / Image / Business / Document)
2. Picks the right agent from the stack
3. Outputs the exact CLI command to run
4. Tells Claude Pro its role as orchestrator

Claude Pro handles: architecture decisions, final reviews, orchestration.  
Everything else goes to: mimo, OpenCode, Gemini CLI, or Codex.

---

## Install

```bash
/plugin marketplace add gagachikhladze/multiagent-cc
```

Then install the free agents:

```bash
/setup
```

---

## Usage

```bash
/delegate write unit tests for auth.py
/delegate research competitors of Lumen Studio photography
/delegate write an Instagram caption for a wedding photo
```

Or call the routing skill directly in any conversation — Claude will use it automatically.

---

## Agent Stack

| Agent | Cost | Best For |
|-------|------|----------|
| **mimo** | Free (early access) | Long-horizon code, 50+ steps |
| **OpenCode Zen Free** | Free | Routine code, non-sensitive |
| **OpenCode Go** | $10/mo | Any task, zero data retention |
| **Gemini CLI** | Free 1,000/day | Research, large docs, fallback |
| **Codex (GPT-5.4/5.5)** | ChatGPT Plus | Code review, adversarial checks |

### Fallback chain (code)
```
mimo → OpenCode Go → OpenCode Zen Free → Gemini CLI → Codex mini → Claude Pro
```

---

## What's included

- `/delegate [task]` — slash command: classify and route any task
- `delegate` skill — full routing logic for all 7 task categories
- `setup` skill — step-by-step install guide for all free agents

---

## What's NOT included

- API credentials or personal keys
- Proxy configuration (personal setup)
- ClickUp/Gmail/Telegram MCP config (install separately)

---

## Supported task categories

- **Code** — mimo, OpenCode Zen/Go, Gemini CLI, Codex
- **Research** — Gemini CLI, WebSearch, deep-research skill
- **Content / Marketing** — OpenCode Go (Georgian + English), PM skills, Meta Ads MCP
- **Image / Video** — Higgsfield MCP
- **Business / PM** — PM skills, ClickUp MCP
- **Communication** — Gmail / Telegram / Calendar MCP
- **Documents** — anthropic-skills (PDF, PPTX, DOCX, XLSX)

---

## Requirements

- Claude Code (CLI or desktop)
- Node.js 18+ (for npm installs)
- Optional: ChatGPT Plus for Codex, OpenCode Go subscription for zero-retention models

---

## License

MIT
