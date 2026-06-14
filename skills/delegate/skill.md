---
name: delegate
description: Route any task to the right free or cheap AI agent. Use this before delegating code, content, research, image/video, business, or document tasks to external CLI agents.
---

# Delegate — Universal Task Routing

Claude Pro acts as **director and orchestrator**. All actual work gets routed to a cheaper or free agent. This skill tells you exactly which tool to use and what command to run.

## Agent Stack

| Agent | Cost | Best For |
|-------|------|----------|
| **mimo** | Free (limited time) | Long-horizon coding, 50+ steps, autonomous runs |
| **OpenCode Zen Free** | Free | Routine coding, non-sensitive work |
| **OpenCode Go** | $12/5h · $30/wk · $60/mo | Paid worker: any language, zero data retention |
| **Gemini CLI** | Free 1,000 req/day | Fallback, Google-grounded research, 1M context |
| **Codex (GPT-5.4/5.5)** | ChatGPT Plus $20/mo | Code review, adversarial checks, critical production code |

---

## Routing Tables

### Code

| Situation | Tool | Model / Command |
|-----------|------|-----------------|
| Long-horizon, 50+ steps | mimo CLI | `mimo "task" --non-interactive` |
| Routine, sensitive data | OpenCode Go | `opencode run "task" --model opencode-go/deepseek-v4-flash` |
| Routine, non-sensitive | OpenCode Zen | `opencode run "task" --model zen/mimo-v2.5-free` |
| Complex algorithm | OpenCode Go | `opencode run "task" --model opencode-go/kimi-k2.7-code` |
| Large codebase (1M ctx) | Gemini CLI | `gemini -p "task" --yolo` |
| Code review | Codex | `/codex:review` |
| Adversarial review | Codex | `/codex:adversarial-review` |
| Critical / production fix | Codex | `/codex:rescue "task"` |

**Note on DeepSeek Free (Zen):** Stores data — use only for non-sensitive/public code. OpenCode Go's DeepSeek is zero-retention.

### Research & Analysis

| Situation | Tool | Command |
|-----------|------|---------|
| Deep multi-source report | Claude: invoke `/deep-research` skill | — |
| Quick single question | WebSearch | — |
| Google-grounded | Gemini CLI | `gemini -p "question"` |
| Large document analysis | Gemini CLI | `gemini` (interactive, paste doc) |
| Competitor analysis | Claude: invoke PM skill | — |

### Content & Marketing

| Situation | Tool | Model |
|-----------|------|-------|
| Social media posts | Claude: invoke `/social-content` skill | — |
| Ad copywriting | Claude: invoke `/copywriting` skill | — |
| SEO content | Claude: invoke `/ai-seo` skill | — |
| Email sequence | Claude: invoke `/email-sequence` skill | — |
| Georgian draft (fast) | OpenCode Go | `opencode run "task" --model opencode-go/deepseek-v4-flash` |
| Georgian draft (quality) | Gemini CLI | `gemini -p "task"` |
| English draft (routine) | OpenCode Go | `opencode run "task" --model opencode-go/qwen3.7-plus` |
| English draft (quality) | OpenCode Go | `opencode run "task" --model opencode-go/qwen3.7-max` |
| Facebook/Meta ads | Claude: use Meta Ads MCP tools | — |

**Georgian language:** Both DeepSeek (OpenCode Go, zero-retention) and Gemini CLI handle Georgian well.

### Image & Video

| Situation | Tool |
|-----------|------|
| Image generation | Higgsfield MCP `generate_image` |
| Video generation | Higgsfield MCP `generate_video` |
| Audio generation | Higgsfield MCP `generate_audio` |
| Product photoshoot | Claude: invoke `/higgsfield-product-photoshoot` skill |
| Virality analysis | Higgsfield MCP `virality_predictor` |
| Image upscale | Higgsfield MCP `upscale_image` |

### Business & PM

| Situation | Tool | Model |
|-----------|------|-------|
| Task management | ClickUp MCP | — |
| PRD / User Stories | Claude: invoke `/pm-execution:write-prd` | — |
| Sprint planning | Claude: invoke `/pm-execution:sprint` | — |
| OKRs | Claude: invoke `/pm-execution:plan-okrs` | — |
| Competitor battlecard | Claude: invoke `/pm-go-to-market:competitive-battlecard` | — |
| Georgian business doc (fast) | OpenCode Go | deepseek-v4-flash |
| English business doc | OpenCode Go | minimax-m2.7 |
| Strategic decisions | Claude Pro | — |

### Communication

| Situation | Tool | Model |
|-----------|------|-------|
| Georgian email draft | OpenCode Go | deepseek-v4-flash |
| English routine email | OpenCode Go | minimax-m2.7 |
| Send email | Gmail MCP | — |
| Telegram message | Telegram MCP | — |
| Calendar event | Calendar MCP | — |

### Documents

| Situation | Tool |
|-----------|------|
| PDF | Claude: invoke `/anthropic-skills:pdf` |
| PPTX presentation | Claude: invoke `/anthropic-skills:pptx` |
| DOCX | Claude: invoke `/anthropic-skills:docx` |
| XLSX | Claude: invoke `/anthropic-skills:xlsx` |

---

## Fallback Chain

**When a tool hits its limit, cascade down:**

```
Code:
  mimo (free)
    → opencode Go + MiMo V2.5 Pro
    → opencode Zen Free (MiMo V2.5 / Nemotron 3 Ultra)
    → Gemini CLI (1,000 req/day)
    → Codex GPT-5.4 mini (60-350 req/5h)
    → Claude Pro (last resort)

Content / Research (Georgian):
  Skill / MCP tool
    → opencode Go + deepseek-v4-flash
    → Gemini CLI
    → Claude Pro (last resort)

Content / Research (English):
  Skill / MCP tool
    → opencode Go + qwen3.7-plus
    → Gemini CLI
    → Claude Pro (last resort)
```

---

## Limits — Quick Reference

| Tool | Limit |
|------|-------|
| mimo | Free for now (MiMo Auto) |
| opencode Zen Free | Free (unknown cap) |
| opencode Go | $12/5h · $30/week · $60/month |
| DeepSeek Flash (Go) | 31,650 req/5h |
| MiMo V2.5 (Go) | 30,100 req/5h |
| Qwen3.7 Plus (Go) | 4,300 req/5h |
| Kimi K2.7 Code (Go) | 1,350 req/5h |
| Gemini CLI | 1,000 req/day · 60/min |
| Codex GPT-5.5 | 15–80 msg/5h |
| Codex GPT-5.4 | 20–100 msg/5h |
| Codex GPT-5.4 mini | 60–350 msg/5h |
| Claude Pro | Monthly Pro limit |

---

## Principles

1. **Skill or MCP first** — specialized tools beat generic agents
2. **Free CLIs for routine code** — mimo, Zen Free, Gemini
3. **OpenCode Go = default paid worker** — DeepSeek Flash for speed, MiMo for quality
4. **Codex = adversarial sparring partner** — review and critical code only
5. **Claude Pro = director + last resort** — orchestrate, don't execute
6. **DeepSeek Zen Free ⚠️** — public/non-sensitive code only (stores data)
7. **Georgian works on DeepSeek Go + Gemini** — not only Claude Pro

---

## Output Format

When routing a task, respond with:

```
CATEGORY: [Code / Research / Content / Image / Business / Document]
TOOL: [tool name]
COMMAND: [exact command to run]
LIMIT STATUS: [Normal / Near limit / Use fallback]
FALLBACK: [next option]
MY ROLE: [what Claude Pro does next]
```
