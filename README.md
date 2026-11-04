# claude-skills

Personal Claude plugin marketplace with one plugin, `rewolwe-skills` (repo root).

Sources:
- Emil Kowalski — https://github.com/emilkowalski/skills (MIT, see LICENSE-emil-kowalski)
- ComposioHQ — https://github.com/ComposioHQ/awesome-claude-skills (see each skill's license)
- `install-from-github` — installs more skills from GitHub links into this repo

Taste skill is installed separately from https://github.com/leonxlnx/taste-skill.

## Install

```
/plugin install rewolwe-skills --marketplace rewolwe/claude-skills
```

In claude.ai: Customize → Plugins → add marketplace `rewolwe/claude-skills`.

## Repo setup
- `CLAUDE.md` — how Claude should maintain this repo.
- `.claude/settings.json` — enables the plugin and pre-approves install commands when the repo is attached to a session.
- `.mcp.json` — add when an MCP server is needed (no secrets; tokens go into environment secrets).
