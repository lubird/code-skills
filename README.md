# code-skills

Personal agent skills following the [Agent Skills specification](https://agentskills.io/specification). Compatible with Claude Code, Codex CLI, and OpenCode.

## Skills

| Skill | Description |
|-------|-------------|
| [obsidian-summarize](skills/obsidian-summarize) | Write a coding session note to an Obsidian vault. Requires the [obsidian-skills](https://github.com/kepano/obsidian-skills) plugin. |

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

`obsidian-summarize` is configured via environment variables:

| Variable | Default | Description |
|----------|---------|-------------|
| `OBSIDIAN_VAULT` | `Second Brain` | Obsidian vault name |
| `OBSIDIAN_NOTES_PATH` | `Coding Sessions` | Folder path within the vault |

Set these per project in your tool's project settings. For Claude Code, add to `.claude/settings.json`:

```json
{
  "env": {
    "OBSIDIAN_NOTES_PATH": "Projects/MyProject"
  }
}
```

## Dependencies

The `obsidian-summarize` skill requires the [kepano/obsidian-skills](https://github.com/kepano/obsidian-skills) plugin for `obsidian:obsidian-cli` and `obsidian:obsidian-markdown`. Install it first:

```
/plugin marketplace add kepano/obsidian-skills
```
