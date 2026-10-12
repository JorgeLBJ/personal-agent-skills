# personal-skills

Personal agent skills for Claude Code and Pi.

| Skill | Purpose |
|-------|---------|
| `change-tour` | Guided stop-by-stop tour of frontend changes, waiting for confirmation between stops |
| `change-report` | Walk changed screens with Playwright and build a local HTML report with screenshots |

## Install

Skills are linked (not copied) into each agent's skill directory:

- `~/.agents/skills/<skill>`
- `~/.claude/skills/<skill>`
- `~/.pi/agent/skills/<skill>`

On Windows (Git Bash), create native symlinks with `MSYS=winsymlinks:nativestrict ln -s <repo>/skills/<skill> <target>`.
