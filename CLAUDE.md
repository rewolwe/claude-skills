# claude-skills — personal Claude setup

This repo is the user's permanent Claude configuration. Cloud containers are
ephemeral: anything not committed here is lost when the session ends.

## Layout
- `.claude-plugin/marketplace.json` — marketplace `rewolwe-skills` with one plugin
  (the repo root). Every skill must be listed in its `skills` array.
- `skills/<name>/SKILL.md` — skills. Folder name == frontmatter `name`.
- `.mcp.json` (create only when needed) — MCP servers, under `mcpServers`. No secrets here.
  The repo root is the plugin root, so this file ships with the plugin.
- `.claude/settings.json` — enables the plugin and pre-approves the git/validate
  commands used for installs when this repo is attached to a session.

## Installing things from GitHub
When the user asks to install / add / поставить / установить a skill, plugin or
connector (or just pastes a GitHub link), follow `skills/install-from-github/SKILL.md`.
Always vet third-party code before copying it in, run `claude plugin validate .`,
bump `metadata.version`, update README.md, then commit and push to `main`.

## Adding an MCP server by hand
```json
{ "mcpServers": { "example": { "type": "http", "url": "https://mcp.example.com/mcp" } } }
```
Keys/tokens go into the cloud environment's secrets and are referenced as
`"headers": { "Authorization": "Bearer ${EXAMPLE_TOKEN}" }`, never committed.
The host must also be allowed in the environment's Network access settings.
