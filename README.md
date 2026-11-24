# AI

Reusable AI agents and skills for software-development assistants.

## Folder locations

The table lists the common project and user locations for custom agents and
Agent Skills. Paths beginning with `~` apply to the current operating-system
user. Project paths are relative to the repository or workspace root.

| AI tool | Agents (project) | Agents (user) | Skills (project) | Skills (user) |
| --- | --- | --- | --- | --- |
| GitHub Copilot (VS Code / CLI) | `.github/agents/*.agent.md` | `~/.copilot/agents/*.agent.md` | `.github/skills/<skill>/SKILL.md` or `.agents/skills/<skill>/SKILL.md` | `~/.copilot/skills/<skill>/SKILL.md` or `~/.agents/skills/<skill>/SKILL.md` |
| Claude Code | `.claude/agents/*.md` | `~/.claude/agents/*.md` | `.claude/skills/<skill>/SKILL.md` | `~/.claude/skills/<skill>/SKILL.md` |
| OpenAI Codex | `.agents/agents/*.md` or `.codex/agents/*.md` | `~/.agents/agents/*.md` or `~/.codex/agents/*.md` | `.agents/skills/<skill>/SKILL.md` | `~/.agents/skills/<skill>/SKILL.md` |
| Gemini CLI | `.gemini/agents/*.md` | `~/.gemini/agents/*.md` | `.gemini/skills/<skill>/SKILL.md` | `~/.gemini/skills/<skill>/SKILL.md` |

### Notes

- Custom-agent file formats and frontmatter fields are tool-specific. Prefer
  copying an agent into the folder for the intended tool instead of assuming
  that an agent written for one tool works unchanged in another.
- Agent Skills use a standard `SKILL.md` structure, but supported folders,
  discovery rules, and optional features vary by tool. Keep a skill in
  `.agents/skills` when it should be shared across compatible coding agents.
- Project-level customizations are normally checked into source control and
  apply to a repository or workspace. User-level customizations apply across
  projects for the current user.
- Documentation and product support can change quickly. Recheck the official
  documentation.