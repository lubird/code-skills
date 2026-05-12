# code-skills

Personal agent skills following the [Agent Skills specification](https://agentskills.io/specification). Compatible with Claude Code, Codex CLI, and OpenCode.

## Skills

| Skill | Description |
|-------|-------------|
| [obsidian-summarize](skills/obsidian-summarize) | Write a coding session note to an Obsidian vault. Requires the [obsidian-skills](https://github.com/kepano/obsidian-skills) plugin. |
| [obsidian-load-context](skills/obsidian-load-context) | Load context from recent session notes at the start of a session. Requires the [obsidian-skills](https://github.com/kepano/obsidian-skills) plugin. |

## Installation

### Claude Code
```
/plugin marketplace add lubird/code-skills
```

### Codex CLI
```
npx skills add https://github.com/lubird/code-skills
```

### OpenCode
```sh
git clone https://github.com/lubird/code-skills.git ~/.opencode/skills/code-skills
```

## Configuration

`obsidian-summarize` and `obsidian-load-context` are configured with a project-local `.agents/code-skills.json` file at the git root:

```json
{
  "obsidian": {
    "vault": "Second Brain",
    "notesPath": "Projects/MyProject"
  }
}
```

Configuration precedence:

| Source | Description |
|--------|-------------|
| `.agents/code-skills.json` | Preferred per-project configuration |
| `OBSIDIAN_VAULT`, `OBSIDIAN_NOTES_PATH` | Optional environment variable fallback |
| Defaults | Vault `Second Brain`, path `Coding Sessions` |

In sandboxed agents, the Obsidian CLI may need to run outside the sandbox to reach Obsidian's local CLI socket.

Example for an Otis project:

```json
{
  "obsidian": {
    "vault": "Second Brain",
    "notesPath": "Projects/Otis"
  }
}
```

## Dependencies

The `obsidian-summarize` skill requires the [kepano/obsidian-skills](https://github.com/kepano/obsidian-skills) plugin for `obsidian:obsidian-cli` and `obsidian:obsidian-markdown`. Install it first:

```
/plugin marketplace add kepano/obsidian-skills
```
