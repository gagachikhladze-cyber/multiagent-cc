---
name: delegate
description: Route any task to the right free AI agent and EXECUTE it automatically. Use this before any coding, content, research, image/video, or business task.
---

# Delegate — Auto-Execute Tasks via Free AI Agents

Claude Pro acts as **director**. You classify the task, pick the agent, run the command via Bash tool, and return the result. Do NOT just describe what to do — actually DO it.

## Execution Protocol

For every delegated task:
1. Classify: Code / Content / Research / Image / Business / Document
2. Pick agent from routing table below
3. **Run the command via Bash tool** — do not ask permission
4. Return the result to the user
5. If agent fails → cascade to next in fallback chain

---

## Routing & Commands

### Code

| Situation | Command to Run |
|-----------|----------------|
| Routine, non-sensitive | `opencode run "<task>" -m opencode/mimo-v2.5-free --dangerously-skip-permissions` |
| Routine, sensitive data | `opencode run "<task>" -m opencode-go/deepseek-v4-flash --dangerously-skip-permissions` |
| Complex algorithm | `opencode run "<task>" -m opencode-go/kimi-k2.7-code --dangerously-skip-permissions` |
| Quality code / quality | `opencode run "<task>" -m opencode-go/mimo-v2.5-pro --dangerously-skip-permissions` |
| Large codebase research | `gemini -p "<task>" --skip-trust --yolo` |

### Research & Analysis

| Situation | Command to Run |
|-----------|----------------|
| Quick question | `gemini -p "<question>" --skip-trust` |
| Deep research | `gemini -p "<question>" --skip-trust` |
| Google-grounded | `gemini -p "<question>" --skip-trust` |

### Content & Marketing

| Situation | Command to Run |
|-----------|----------------|
| Georgian text (fast) | `opencode run "<task>" -m opencode-go/deepseek-v4-flash --dangerously-skip-permissions` |
| Georgian text (quality) | `gemini -p "<task>" --skip-trust` |
| English text (routine) | `opencode run "<task>" -m opencode-go/qwen3.7-plus --dangerously-skip-permissions` |
| English text (quality) | `opencode run "<task>" -m opencode-go/qwen3.7-max --dangerously-skip-permissions` |
| Social media / copywriting | Claude: invoke relevant skill (`/social-content`, `/copywriting`, etc.) |
| Facebook/Meta ads | Claude: use Meta Ads MCP tools |

### Image & Video

| Situation | Action |
|-----------|--------|
| Image generation | Claude: call Higgsfield MCP `generate_image` |
| Video generation | Claude: call Higgsfield MCP `generate_video` |
| Product photoshoot | Claude: invoke `/higgsfield-product-photoshoot` skill |

### Business / PM

| Situation | Action |
|-----------|--------|
| Task management | Claude: use ClickUp MCP |
| PRD, OKRs, Sprint | Claude: invoke relevant PM skill |
| Georgian business doc | `opencode run "<task>" -m opencode-go/deepseek-v4-flash --dangerously-skip-permissions` |

### Communication

| Situation | Action |
|-----------|--------|
| Gmail | Claude: use Gmail MCP |
| Telegram | Claude: use Telegram MCP |
| Calendar | Claude: use Calendar MCP |

### Documents

| Situation | Action |
|-----------|--------|
| PDF / PPTX / DOCX / XLSX | Claude: invoke `/anthropic-skills:pdf/pptx/docx/xlsx` |

---

## Fallback Chain

If a command fails or hits limit, try next:

```
Code:
  opencode/mimo-v2.5-free (Zen, free)
    → opencode-go/deepseek-v4-flash (Go, paid, zero-retention)
    → opencode-go/mimo-v2.5-pro (Go)
    → gemini --skip-trust (free 1,000/day)
    → Claude Pro (last resort)

Content/Research (Georgian):
  opencode-go/deepseek-v4-flash
    → gemini --skip-trust
    → Claude Pro

Content/Research (English):
  opencode-go/qwen3.7-plus
    → gemini --skip-trust
    → Claude Pro
```

---

## Critical Rules

- **Always execute via Bash tool** — never just print the command
- **`--dangerously-skip-permissions`** on all `opencode run` calls — prevents interactive prompts
- **`--skip-trust`** on all `gemini` calls — required for non-interactive use
- **DeepSeek Zen Free (`opencode/deepseek-v4-flash-free`)** ⚠️ — stores data, use only for public/non-sensitive tasks
- **Georgian language** — DeepSeek Go and Gemini both handle it well
- Pipe long outputs through `| tail -100` if context is large

---

## Limits Reference

| Tool | Limit |
|------|-------|
| opencode/mimo-v2.5-free | Free (unknown cap) |
| opencode-go/deepseek-v4-flash | 31,650 req/5h |
| opencode-go/mimo-v2.5-pro | 30,100 req/5h |
| opencode-go/qwen3.7-plus | 4,300 req/5h |
| opencode-go/kimi-k2.7-code | 1,350 req/5h |
| gemini --skip-trust | 1,000 req/day |
| Claude Pro | Monthly Pro limit |
