---
name: setup
description: Auto-check and install all required CLI agents (opencode, gemini, mimo). Run this once before first use, or call it anytime to repair broken tools.
---

# Setup — Auto-Install All Agents

Run ALL checks in parallel, install anything missing, verify everything works.

## Full Setup Sequence

### 1. Check what's installed

```bash
opencode --version 2>&1
gemini --version 2>&1
mimo --version 2>&1
node --version 2>&1
npm --version 2>&1
```

### 2. Install missing tools

**If opencode missing:**
```bash
npm install -g opencode-ai@latest
```

**If opencode version < 1.17.0 (has run bug):**
```bash
npm install -g opencode-ai@latest
```

**If gemini missing:**
```bash
npm install -g @google/gemini-cli@latest
```

**If mimo missing:**
```bash
npm install -g @mimo-ai/cli
```
If that fails:
```bash
curl -fsSL https://mimo.xiaomi.com/install | bash
```

### 3. Verify auth

**opencode providers:**
```bash
opencode providers list 2>&1
```
Expected: at least "OpenCode Go" or "OpenCode Zen" listed.
If empty: tell user to run `opencode` in terminal once and connect via `/connect`.

**gemini auth:**
```bash
gemini -p "hello" --skip-trust 2>&1
```
If fails with auth error: tell user to run `gemini` in terminal once and log in with Google account.

**mimo first run (if installed):**
```bash
mimo --version 2>&1
```
If installed but never configured: tell user to run `mimo` once and select "MiMo Auto" model.

### 4. Run smoke test

After all installs, verify execution works:

```bash
opencode run "say hello" -m opencode-go/deepseek-v4-flash --dangerously-skip-permissions 2>&1
gemini -p "say hello" --skip-trust 2>&1
```

Both should return a response. Report results to user.

### 5. Report status

Show a clear summary:

```
✓ opencode v1.17.x — ready (Go + Zen connected)
✓ gemini v0.42.x — ready (Google auth OK)
✗ mimo — not installed (using opencode as fallback)

Ready to use /delegate
```

---

## What Requires Manual Steps

These cannot be automated (need browser):
- **gemini first login** — run `gemini` in terminal → Google OAuth
- **opencode Go/Zen connect** — run `opencode` in terminal → `/connect` → login
- **mimo first model select** — run `mimo` in terminal → select "MiMo Auto"

Everything else installs automatically.
