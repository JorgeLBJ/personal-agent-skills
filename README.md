# personal-agent-skills

Personal agent skills for Claude Code, OpenCode, and Pi.

| Skill | Purpose |
|-------|---------|
| `change-tour` | Guided stop-by-stop tour of frontend changes, waiting for confirmation between stops |
| `change-report` | Walk changed screens with Playwright and build a local HTML report with screenshots |

## Install

### Quick install (copy)

Uses the [`skills`](https://github.com/vercel-labs/skills) CLI. It asks which agents to install into.

```bash
# All skills
npx skills add JorgeLBJ/personal-agent-skills

# A single skill
npx skills add JorgeLBJ/personal-agent-skills@change-tour
npx skills add JorgeLBJ/personal-agent-skills@change-report
```

Update later with `npx skills update`.

### Development install (symlinks)

Use this on machines where you edit the skills, so changes go through git.

```bash
git clone https://github.com/JorgeLBJ/personal-agent-skills.git
cd personal-agent-skills

# Windows (Git Bash) needs native symlinks; requires Developer Mode or an elevated shell
export MSYS=winsymlinks:nativestrict

for dir in ~/.agents/skills ~/.claude/skills ~/.pi/agent/skills ~/.config/opencode/skills; do
  mkdir -p "$dir"
  for skill in skills/*/; do
    ln -s "$PWD/$skill" "$dir/$(basename "$skill")"
  done
done
```

| Agent | Skill directory |
|-------|-----------------|
| Shared | `~/.agents/skills/` |
| Claude Code | `~/.claude/skills/` |
| Pi | `~/.pi/agent/skills/` |
| OpenCode | `~/.config/opencode/skills/` |

## Requirements

- `change-report` requires the Playwright MCP server.
- `change-tour` uses Playwright MCP when available and falls back to routes and manual steps.

## Usage

- `/change-tour` or "give me a tour of the changes"
- `/change-report` or "build the visual report of the change"
