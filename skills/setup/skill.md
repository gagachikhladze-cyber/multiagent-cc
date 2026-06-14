---
name: setup
description: Install and configure the free AI CLI agents needed for the multi-agent delegation stack — mimo, Gemini CLI, OpenCode, and the Codex plugin.
---

# Setup — Install Free AI Agents

This skill walks you through installing every agent in the multi-agent stack. Run these once. After setup, use `/delegate` to route tasks automatically.

---

## 1. mimo CLI — Free Coding Agent

**What it does:** Long-horizon autonomous coding (50+ steps), 1M context, memory system, parallel execution. Built on OpenCode. Currently free (MiMo Auto model).

```bash
# Option A — curl
curl -fsSL https://mimo.xiaomi.com/install | bash

# Option B — npm
npm install -g @mimo-ai/cli
```

**First run:**
```bash
mimo
# → "Select model access": choose "MiMo Auto" (free)
```

**Usage:**
```bash
mimo "task description"                    # interactive
mimo "task description" --non-interactive  # autonomous
```

**Why use it:** Outperforms Claude Code + Sonnet 4.6 on long coding benchmarks. Free while in early access.

---

## 2. Gemini CLI — Free Research + Fallback

**What it does:** 1,000 free requests/day, 1M context, Google Search grounding. Best for research, large document analysis, and as a coding fallback.

```bash
npm install -g @google/gemini-cli
```

**Auth (one time):**
```bash
gemini
# → Browser opens → sign in with Google account
```

**Usage:**
```bash
gemini -p "question or task"       # single prompt
gemini -p "task" --yolo            # auto-approve tool calls
gemini                             # interactive TUI
```

**Limits:** 60 req/min · 1,000 req/day · Free

---

## 3. OpenCode — Multi-Model CLI

**What it does:** Routes tasks to 75+ AI models. Two tiers: Zen Free (no registration) and Go ($10/mo, zero data retention).

```bash
npm install -g opencode-ai
```

### Zen Free — No Registration Required

Free models, auto-available after install:

```bash
opencode
# In TUI: /connect → "OpenCode Zen" → opencode.ai/auth → sign in
# Then: /models → select:
#   - MiMo V2.5 Free     ← best free coding model
#   - Nemotron 3 Ultra Free
#   - North Mini Code Free
```

**Usage:**
```bash
opencode run "task" --model zen/mimo-v2.5-free
```

**Note:** DeepSeek Flash Free is also available but **stores data** — use only for non-sensitive/public code.

### Go ($10/mo, $5 first month) — Zero Data Retention

All models have zero data retention. Best paid worker.

```bash
opencode
# /connect → "OpenCode Go" → opencode.ai/auth → subscribe → API key
```

**Key models:**
```bash
# Fast + cheap (Georgian works well)
opencode run "task" --model opencode-go/deepseek-v4-flash

# Balanced quality
opencode run "task" --model opencode-go/mimo-v2.5-pro

# Complex algorithms
opencode run "task" --model opencode-go/kimi-k2.7-code

# English content quality
opencode run "task" --model opencode-go/qwen3.7-plus
```

**Limits:** $12/5h · $30/week · $60/month

---

## 4. Codex Plugin — GPT-5.4 / 5.5 via Claude Code

**What it does:** Brings OpenAI's Codex (GPT-5.4, GPT-5.5) into Claude Code. Best for code review, adversarial checks, and critical production fixes.

**Requires:** ChatGPT Plus or Pro ($20/mo)

```bash
# In Claude Code terminal:
/plugin marketplace add openai/codex-plugin-cc
/plugin install codex@openai-codex
/reload-plugins
/codex:setup
```

**If login is needed:**
```bash
!codex login
```

**Commands:**
```bash
/codex:review                              # code review
/codex:review --base main                  # compare vs branch
/codex:adversarial-review                  # challenge your design
/codex:rescue "fix the failing tests"      # delegate a task
/codex:rescue --background "investigate"   # async background task
/codex:rescue --model gpt-5.4-mini "task" # cheaper model
/codex:status                              # check job status
/codex:result                             # get result
```

**Limits:**
- GPT-5.5: 15–80 msg/5h
- GPT-5.4: 20–100 msg/5h
- GPT-5.4 mini: 60–350 msg/5h

---

## Verification

After installing all tools, verify:

```bash
mimo --version
gemini --version
opencode --version
```

Then in Claude Code, type `/delegate write a Python function that...` and confirm routing works.

---

## Summary

| Tool | Install | Cost |
|------|---------|------|
| mimo | `npm install -g @mimo-ai/cli` | Free |
| Gemini CLI | `npm install -g @google/gemini-cli` | Free 1,000/day |
| OpenCode Zen | `npm install -g opencode-ai` + /connect Zen | Free |
| OpenCode Go | same + /connect Go + subscribe | $10/mo |
| Codex plugin | `/plugin marketplace add openai/codex-plugin-cc` | ChatGPT Plus $20/mo |
