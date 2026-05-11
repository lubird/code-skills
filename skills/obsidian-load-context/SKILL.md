---
name: obsidian-load-context
description: Load context from recent coding session notes in an Obsidian vault. Use at the start of a session to catch up on recent work, open items, and next steps. Requires Obsidian to be open and the obsidian:obsidian-cli skill.
allowed-tools: Bash(obsidian:*)
---

Load the last N session notes from an Obsidian vault and synthesize a context briefing for the current session.

Vault and path are configurable via environment variables:
- `OBSIDIAN_VAULT` — vault name (default: `Second Brain`)
- `OBSIDIAN_NOTES_PATH` — folder path within the vault (default: `Coding Sessions`)

The number of sessions to load defaults to 3 but can be overridden at call time.

## Steps

1. **Find recent notes** using the `obsidian:obsidian-cli` skill. Search for notes in the configured folder, sorted by recency, limited to N:
   ```bash
   obsidian vault="${OBSIDIAN_VAULT:-Second Brain}" search query="path:\"${OBSIDIAN_NOTES_PATH:-Coding Sessions}\"" limit=<N>
   ```

2. **Read each note**:
   ```bash
   obsidian vault="${OBSIDIAN_VAULT:-Second Brain}" read path="<note path>"
   ```

3. **Synthesize a briefing** from the notes and present it to the user. Structure it as:

   ### Recent Work
   <Narrative summary across all loaded sessions — what was built, what changed, key decisions made>

   ### Open Items
   <Consolidated list of open items and deferred work from all sessions, most recent first>

   ### Next Steps
   <Consolidated next steps from all sessions, most recent first. Flag any that appear across multiple sessions.>

4. **Report back**: state how many notes were loaded and their date range.
