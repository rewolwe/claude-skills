---
name: install-from-github
description: Install a skill, plugin or MCP connector from a GitHub link into the user's permanent plugin marketplace repo rewolwe/claude-skills. Use whenever the user pastes a GitHub URL and asks to install / add / set up / поставить / установить a skill, plugin or connector, or just sends a GitHub link to a skills repo with no other instruction.
---

# Install from GitHub

The user keeps every extra skill in the private repo **rewolwe/claude-skills**, which is
added to their claude.ai account as a plugin marketplace (`rewolwe-skills`). Anything pushed
into the `my-skills` plugin there reaches all of their sessions after the marketplace syncs.
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
     `plugins/my-skills/skills/<name>/`. The folder name must equal the frontmatter `name`
     (lowercase, digits, hyphens, ≤64 chars); description ≤1024 chars. Rename the folder if needed.
   - *A repo with hundreds of skills*: don't import all of them. Install the general-purpose
     ones and ask the user which of the rest they want.
   - *A Claude plugin / marketplace* (`.claude-plugin/`): if it is a marketplace, tell the user
     they can add that repo directly in claude.ai (Customize → Plugins) — that keeps upstream
     updates. If they prefer, vendor its skills into `my-skills` as above.
   - *A remote MCP connector* (an HTTPS MCP endpoint): add it to `plugins/my-skills/.mcp.json`
     under `mcpServers` with `"type": "http"` and its `url`. Never put secrets in the repo; if it
     needs a key, tell the user to add it as a custom connector in claude.ai instead.
   - *A local-only (stdio) MCP server*: it cannot run in claude.ai cloud — say so and suggest
     running it on their own machine.
   - Skip anything that duplicates a skill they already have; say which.
5. **Register.** Update `skills` in `plugins/my-skills/.claude-plugin/plugin.json` and in the
   `my-skills` entry of `.claude-plugin/marketplace.json`; bump `version` (patch) in both.
   Add a line to README.md with the source URL. Run `claude plugin validate .`.
6. **Commit and push** to `main` (fetch first). Verify `git ls-remote origin main` matches HEAD.
7. **Also copy** the new skills into `~/.claude/skills/` so they work in this session right away.
8. **Tell the user** in their language: what was installed, what was skipped and why, any keys
   or tools needed, and that new sessions get it once the `rewolwe-skills` marketplace syncs
   in claude.ai (Customize → Plugins → sync, if it hasn't updated on its own).
