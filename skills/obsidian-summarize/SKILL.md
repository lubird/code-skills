---
name: obsidian-summarize
description: Write a coding session note to an Obsidian vault summarizing work done, decisions made, and next steps. Use at the end of a coding session. Requires Obsidian to be open and the obsidian:obsidian-cli and obsidian:obsidian-markdown skills.
allowed-tools: Read Bash(date:*) Bash(git log:*) Bash(git rev-parse:*) Bash(basename:*) Bash(obsidian:*)
---

Write a session note for the current coding project to an Obsidian vault using the `obsidian:obsidian-cli` and `obsidian:obsidian-markdown` skills.

Vault and path are configurable per project with `.agents/code-skills.json` at the git root:

```json
{
  "obsidian": {
    "vault": "Second Brain",
    "notesPath": "Projects/MyProject"
  }
}
```

Configuration precedence:
1. `.agents/code-skills.json` fields: `obsidian.vault`, `obsidian.notesPath`
2. Environment variables: `OBSIDIAN_VAULT`, `OBSIDIAN_NOTES_PATH`
3. Defaults: vault `Second Brain`, notes path `Coding Sessions`

## Steps

1. **Gather context** using Bash:
   - Date: `date +"%Y-%m-%d"`
   - Time: `date +"%H:%M"`
   - Project name: `basename $(git rev-parse --show-toplevel)`
   - Recent commits: `git log --oneline -10`

2. **Resolve Obsidian configuration**:
   - Get the git root with `git rev-parse --show-toplevel`.
   - If `<git-root>/.agents/code-skills.json` exists, read it and use `obsidian.vault` and `obsidian.notesPath` from that file.
   - Otherwise use `OBSIDIAN_VAULT` and `OBSIDIAN_NOTES_PATH` from the environment.
   - If neither is set, use vault `Second Brain` and notes path `Coding Sessions`.

3. **Draft the note** using the structure below. Focus only on work done in the current project — exclude meta-tooling, skill setup, or configuration unrelated to the project.

4. **Generate the filename**: `YYYY-MM-DD_<project>_<3-to-5-word-slug>.md`
   - Slug summarizes the session work. Lowercase kebab-case.
   - Example: `2026-05-11_otis_ha-delivery-loop-llm-abstraction.md`

5. **Create the note** using the `obsidian:obsidian-cli` skill. Write content to a temp file first to avoid shell interpretation of backticks and special characters, then pass it to obsidian:
   ```bash
   cat > /tmp/obsidian_note.md << 'NOTEEOF'
   <full note content here>
   NOTEEOF
   obsidian vault="<resolved vault>" create name="YYYY-MM-DD_project_slug" path="<resolved notesPath>/YYYY-MM-DD_project_slug.md" content="$(cat /tmp/obsidian_note.md)" silent
   rm /tmp/obsidian_note.md
   ```
   Follow `obsidian:obsidian-markdown` for correct Obsidian Flavored Markdown syntax.

6. **Report back**: confirm the filename and that the note was written.

---

## Note structure

```markdown
---
date: YYYY-MM-DD
time: HH:MM
project: <project>
tags: [coding-session, <project>]
---

# <project> — YYYY-MM-DD

## Summary
<2–4 sentence narrative of the work done this session. Be specific — name the files, modules, and concepts touched.>

## Key Decisions
- <Notable architectural or design choices, and why>

## Changes Made
- `path/to/file.ts` — <what changed and why>

## Topics Referenced
- <Libraries, concepts, protocols, or tools discussed>

## Open Items
- <Bugs noticed, edge cases deferred, things that need investigation>

## Next Steps
- <What comes next based on conversation context>

## See Also
- [[<related note or prior session if relevant>]]
```
