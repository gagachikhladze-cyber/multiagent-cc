---
name: delegate
description: Pre-flight check tools, auto-install missing ones, execute the task via the right free agent, return the result. Use for any coding, content, research, image/video, or business task.
---

# Delegate — Check → Install → Execute → Return

Claude Pro is the **director**. You handle the full pipeline automatically.

---

## Step 1 — Pre-flight Check (always run first)

Before executing any task, run ALL of these checks in parallel via Bash tool:

```bash
# Check opencode
opencode --version 2>&1

# Check gemini  
gemini --version 2>&1

# Check mimo
mimo --version 2>&1

# Check opencode providers (auth)
opencode providers list 2>&1
```

---

## Step 2 — Auto-Install Missing Tools

For each tool that is missing or errors, install it automatically:

### opencode missing:
```bash
npm install -g opencode-ai@latest
```
Then verify providers are connected:
```bash
opencode providers list 2>&1
```
If providers show 0 credentials, inform user they need to run `opencode` TUI once to connect OpenCode Go/Zen.

### gemini missing:
```bash
npm install -g @google/gemini-cli@latest
```
Then test if auth works:
```bash
gemini -p "hello" --skip-trust 2>&1
```
If auth fails (exit code non-zero), inform user to run `gemini` once in terminal to log in with Google account, then retry.

### mimo missing:
```bash
npm install -g @mimo-ai/cli 2>&1
```
If install fails, try curl install:
```bash
curl -fsSL https://mimo.xiaomi.com/install | bash 2>&1
```
If mimo still unavailable after install, skip and use opencode as fallback — do not block task execution.

### opencode outdated (version < 1.17.0):
```bash
npm install -g opencode-ai@latest
```

---

## Step 3 — Execute Task

After confirming tools are ready, execute based on task type:

### Code

| Situation | Command |
|-----------|---------|
| Routine, non-sensitive | `opencode run "<task>" -m opencode/mimo-v2.5-free --dangerously-skip-permissions` |
| Routine, sensitive data | `opencode run "<task>" -m opencode-go/deepseek-v4-flash --dangerously-skip-permissions` |
| Complex algorithm | `opencode run "<task>" -m opencode-go/kimi-k2.7-code --dangerously-skip-permissions` |
| Quality / production | `opencode run "<task>" -m opencode-go/mimo-v2.5-pro --dangerously-skip-permissions` |
| Long-horizon, 50+ steps | `mimo "<task>" --non-interactive` (if installed) |
| Large codebase research | `gemini -p "<task>" --skip-trust --yolo` |

### Research

| Situation | Command |
|-----------|---------|
| Any research question | `gemini -p "<question>" --skip-trust` |
| Deep / multi-source | `gemini -p "<question>" --skip-trust` |

### Content & Marketing

| Situation | Command |
|-----------|---------|
| Georgian text (fast) | `opencode run "<task>" -m opencode-go/deepseek-v4-flash --dangerously-skip-permissions` |
| Georgian text (quality) | `gemini -p "<task>" --skip-trust` |
| English text | `opencode run "<task>" -m opencode-go/qwen3.7-plus --dangerously-skip-permissions` |
| Social media / copywriting | Invoke relevant skill (`/social-content`, `/copywriting`, etc.) |
| Facebook/Meta ads | Use Meta Ads MCP tools |

### Image / Video

| Situation | Action |
|-----------|--------|
| Image | Call Higgsfield MCP `generate_image` |
| Video | Call Higgsfield MCP `generate_video` |
| Product photoshoot | Invoke `/higgsfield-product-photoshoot` skill |

### Business / PM

| Situation | Action |
|-----------|--------|
| Tasks | Use ClickUp MCP |
| PRD, OKRs, Sprint | Invoke relevant PM skill |
| Georgian business doc | `opencode run "<task>" -m opencode-go/deepseek-v4-flash --dangerously-skip-permissions` |

### Communication

| Situation | Action |
|-----------|--------|
| Gmail | Use Gmail MCP |
| Telegram | Use Telegram MCP |
| Calendar | Use Calendar MCP |

### Documents

| Situation | Action |
|-----------|--------|
| PDF / PPTX / DOCX / XLSX | Invoke `/anthropic-skills:pdf/pptx/docx/xlsx` |

---

## Step 4 — Return Result

After execution:
- Return the full agent output to the user
- If the result is code: show it directly
- If the result is text/content: show it directly
- If execution failed: show error + run fallback automatically

---

## Fallback Chain (auto-cascade on failure)

```
Code:
  opencode/mimo-v2.5-free
    → opencode-go/deepseek-v4-flash
    → opencode-go/mimo-v2.5-pro
    → gemini -p "..." --skip-trust
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

On any command failure: catch the error, log it, try next in chain automatically.

---

## Rules

- **Always run pre-flight check first** — every single time
- **Always execute via Bash tool** — never just print commands
- **`--dangerously-skip-permissions`** on all `opencode run` calls
- **`--skip-trust`** on all `gemini` calls
- **DeepSeek Zen Free** ⚠️ — stores data, public/non-sensitive only
- **Georgian** — DeepSeek Go and Gemini both work
- Pipe long outputs through `| tail -200` if needed

---

## Limits Reference

| Tool | Limit |
|------|-------|
| opencode/mimo-v2.5-free | Free |
| opencode-go/deepseek-v4-flash | 31,650 req/5h |
| opencode-go/mimo-v2.5-pro | 30,100 req/5h |
| opencode-go/qwen3.7-plus | 4,300 req/5h |
| opencode-go/kimi-k2.7-code | 1,350 req/5h |
| gemini | 1,000 req/day |
| Claude Pro | Monthly Pro limit |
