---
name: install-from-github
description: Install a skill, plugin or MCP connector from a GitHub link into the user's permanent plugin marketplace repo rewolwe/claude-skills. Use whenever the user pastes a GitHub URL and asks to install / add / set up / поставить / установить a skill, plugin or connector, or just sends a GitHub link to a skills repo with no other instruction.
---

# Install from GitHub

The user keeps every extra skill in the public repo **rewolwe/claude-skills**, which is
added to their claude.ai account as a plugin marketplace (`rewolwe-skills`). The repo root is a single plugin, also named `rewolwe-skills`; anything pushed into its `skills/` folder reaches all of their sessions after the marketplace syncs.
Never install only into `~/.claude/skills` — that dies with the container.

## Steps

1. **Attach the marketplace repo.** Call `add_repo` with owner `rewolwe`, repo `claude-skills`,
   access `push`, then clone it as the tool result says.
2. **Fetch the source.** Clone the linked repo with `--depth 1` into a fresh empty directory in
   the scratchpad. Do not run anything from it.
3. **Vet it before installing.** Read every SKILL.md and skim every script. Flag and stop to ask
   if anything downloads and runs code, reads credentials or env vars, sends data to unknown
   hosts, or is obfuscated. Note what needs API keys or extra tools.
4. **Decide what it is:**
   - *Skill folders* (`SKILL.md` with `name` + `description` frontmatter): copy each into
     `skills/<name>/`. The folder name must equal the frontmatter `name`
     (lowercase, digits, hyphens, ≤64 chars); description ≤1024 chars. Rename the folder if needed.
   - *A repo with hundreds of skills*: don't import all of them. Install the general-purpose
     ones and ask the user which of the rest they want.
   - *A Claude plugin / marketplace* (`.claude-plugin/`): if it is a marketplace, tell the user
     they can add that repo directly in claude.ai (Customize → Plugins) — that keeps upstream
     updates. If they prefer, vendor its skills into `skills/` as above.
   - *A remote MCP connector* (an HTTPS MCP endpoint): add it to `.mcp.json` at the repo root
     under `mcpServers` with `"type": "http"` and its `url` (create the file if missing; then
     re-check that the claude.ai marketplace still syncs). Never put secrets in the repo; if it
     needs a key, tell the user to add it as a custom connector in claude.ai instead.
   - *A local (stdio) MCP server* (`npx`/`uvx`/`docker` command): it works only in Claude Code
     sessions (CLI or claude.ai/code cloud), not in claude.ai chat. Add it to `.mcp.json` with
     `command`/`args`; env vars as `${VAR}` placeholders, with the values saved by the user in
     the cloud environment's secrets. Say that limitation to the user.
   - Skip anything that duplicates a skill they already have; say which.
5. **Register.** Add the new paths to the `skills` list of the single plugin entry in
   `.claude-plugin/marketplace.json` (keep it sorted) and bump `metadata.version` (patch).
   Keep the manifest in the same minimal shape as anthropics/skills (`source: "./"`,
   `strict: false`, no extra per-plugin fields) — claude.ai sync is stricter than the CLI.
   Add a line to README.md with the source URL. Run `claude plugin validate .`.
6. **Commit and push** to `main` (fetch first). Verify `git ls-remote origin main` matches HEAD.
7. **Also copy** the new skills into `~/.claude/skills/` so they work in this session right away.
8. **Tell the user** in their language: what was installed, what was skipped and why, any keys
   or tools needed, and that new sessions get it once the `rewolwe-skills` marketplace syncs
   in claude.ai (Customize → Plugins → sync, if it hasn't updated on its own).
