# Instructions for AI Coding Agents

**For Claude Code, Codex, Gemini, or any AI coding assistant.**

## Quick Setup (Auto-Prompt Hook)

Add a SessionStart hook so Claude automatically prompts in unconfigured directories.

**1. Create** `~/.claude/hooks/check-launchpad.sh`:

```bash
#!/bin/bash
if [[ "$PWD" == "$HOME" ]] || [[ "$PWD" == "/" ]] || [[ "$PWD" == /tmp* ]]; then exit 0; fi
if [[ ! -f "AGENTS.md" ]] && [[ ! -f "CLAUDE.md" ]] && [[ ! -f ".claude/CLAUDE.md" ]]; then
    echo "📋 This project isn't configured. Set up with Launchpad?"
    echo "  curl -fsSL https://raw.githubusercontent.com/kjosh15/launchpad/main/bootstrap.sh | bash"
fi
exit 0
```

**2. Make executable:** `chmod +x ~/.claude/hooks/check-launchpad.sh`

**3. Add to** `~/.claude/settings.json`:

```json
{
  "hooks": {
    "SessionStart": [{"hooks": [{"type": "command", "command": "~/.claude/hooks/check-launchpad.sh"}]}]
  }
}
```

---

## What is Launchpad?

Launchpad is a lean, TDD-focused configuration template for AI coding agents. It provides:

- **AGENTS.md** - Core process rules (TDD ratchet, SuperPowers integration)
- **PROJECT_CONTEXT.md** - Template for project-specific facts
- **Progressive disclosure** - Architecture and testing rules in `.claude/rules/`
- **~380 lines total** - No bloat, low token cost

---

## Setup Instructions

### Option 1: New Project (Use as Template)

```bash
# Clone launchpad
git clone https://github.com/kjosh15/launchpad my-project-name
cd my-project-name

# Reset git history for fresh start
rm -rf .git
git init
git add -A
git commit -m "Initial commit from launchpad template"
```

### Option 2: Existing Project (Bootstrap)

```bash
cd /path/to/existing/project

# Run bootstrap script
curl -fsSL https://raw.githubusercontent.com/kjosh15/launchpad/main/bootstrap.sh | bash
```

### Option 3: Manual Copy

Download these files to your project root:
- `CLAUDE.md`
- `AGENTS.md`
- `PROJECT_CONTEXT.md`
- `.claudeignore`
- `.claude/rules/architecture.md`
- `.claude/rules/testing.md`

---

## After Setup

1. **Edit `PROJECT_CONTEXT.md`** - Fill in your project's commands, ports, and stack
2. **Start coding** - The TDD workflow activates automatically

The agent will refuse to proceed if `PROJECT_CONTEXT.md` still has `[TODO]` placeholders.

---

## Requirements

- [SuperPowers plugin](https://github.com/obra/superpowers) for full workflow support
- Claude Code or OpenAI Codex

---

## The Workflow

Once configured, the agent follows this process:

```
PLAN → RED → GREEN → REFACTOR → VERIFY
```

- **Before coding**: Brainstorming skill refines design
- **During coding**: TDD enforced (test first, then implement)
- **After coding**: Code review + verification before claiming done

---

**Repository:** https://github.com/kjosh15/launchpad
