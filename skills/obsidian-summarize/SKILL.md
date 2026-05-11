---
name: obsidian-summarize
description: Write a coding session note to an Obsidian vault summarizing work done, decisions made, and next steps. Use at the end of a coding session. Requires Obsidian to be open and the obsidian:obsidian-cli and obsidian:obsidian-markdown skills.
allowed-tools: Bash(date:*) Bash(git log:*) Bash(git rev-parse:*) Bash(basename:*) Bash(obsidian:*)
---

Write a session note for the current coding project to an Obsidian vault using the `obsidian:obsidian-cli` and `obsidian:obsidian-markdown` skills.

Vault and path are configurable via environment variables:
- `OBSIDIAN_VAULT` — vault name (default: `Second Brain`)
- `OBSIDIAN_NOTES_PATH` — folder path within the vault (default: `Coding Sessions`)

## Steps

1. **Gather context** using Bash:
   - Date: `date +"%Y-%m-%d"`
   - Time: `date +"%H:%M"`
   - Project name: `basename $(git rev-parse --show-toplevel)`
   - Recent commits: `git log --oneline -10`

2. **Draft the note** using the structure below. Focus only on work done in the current project — exclude meta-tooling, skill setup, or configuration unrelated to the project.

3. **Generate the filename**: `YYYY-MM-DD_<project>_<3-to-5-word-slug>.md`
   - Slug summarizes the session work. Lowercase kebab-case.
   - Example: `2026-05-11_otis_ha-delivery-loop-llm-abstraction.md`

4. **Create the note** using the `obsidian:obsidian-cli` skill:
   ```bash
   obsidian vault="${OBSIDIAN_VAULT:-Second Brain}" create name="YYYY-MM-DD_project_slug" path="${OBSIDIAN_NOTES_PATH:-Coding Sessions}/YYYY-MM-DD_project_slug.md" content="<note content>" silent
   ```
   Use `\n` for newlines in the content string. Follow `obsidian:obsidian-markdown` for correct Obsidian Flavored Markdown syntax.

5. **Report back**: confirm the filename and that the note was written.

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
